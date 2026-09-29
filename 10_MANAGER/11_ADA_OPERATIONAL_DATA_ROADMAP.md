# ADA Datos operacionales — contratos abiertos y roadmap de incrementos

Estado: **CURRENT PLANNING / 2026-09-29**. Este documento ordena únicamente el frente de Datos operacionales. No sustituye prioridades vigentes de KPI, Alarm Engine, Master, distribución ni otras líneas de trabajo.

## Referencias de partida

```text
Implementation verified:   moragaga/atlanticus:main@caced5d7711cf059d36ec61aecc9b3e9629bd41f
Canonical before candidate: moragaga/atlanticus-cannonical:main@ec16bd2ccf0ae06065b8ee1d3a231ef4d2cbac57
Tests reported:             operational-identification 25 passed; Manager 13 passed in a separate gate
Ruff reported:              I001 in operational-identification/service.py; NOT PASS
```

**Histórico SUPERSEDED:** «Backend operacional PLANNED / NO IMPLEMENTATION», «users-support solo candidato sin evidencia» y «Asignaciones/Cargos pendientes de cualquier integración Manager». El código ya incluye backend, Source separados, proyecciones y Manager. **No** convertir la nueva UX, el snapshot consolidado, sesión Entra o warmup en CURRENT.

## Contrato funcional DECIDED

1. Sección administrativa **Datos operacionales**; primera pestaña **Datos operacionales** con cargos y referencias informativas Mina/Planta, grupos 1–4; segunda pestaña **Asignación**, con promovidos y modal individual.
2. Guardar en Source durable, proyectar documento individual o de catálogo en Cosmos y mostrar estado/trazabilidad/reintento bajo patrones Atlanticus. No alterar generic Users, perfiles ni accesos.
3. Runtime y workers leen **solo Cosmos**, nunca Blob. Las asignaciones individuales conservan IDs; el catálogo compartido resuelve etiquetas y metadatos, evitando escrituras masivas cuando cambia una etiqueta de cargo.
4. El inicio/recarga de sesión resuelve identidad/promoción; el usuario no promovido es `guest`. Tras promoción se requiere recarga de página. La asignación individual se consulta bajo demanda en Cosmos para esa sesión.
5. El warmup de cada proceso solo incorpora **Profiles** y **catálogo operacional**, con refresco periódico; **no** precarga usuarios ni sus asignaciones. Tampoco determina promociones ni resuelve autorización por sí mismo.

## DECIDED — significado de «snapshot único en Storage»

**VERIFIED:** la implementación actual publica **un Source por usuario** y otro Source independiente para catálogo, cada uno con su propio historial. No existe aún un consolidado. `SourceStore` no enumera todas las claves publicadas y `UsersAdministrationStore.list_users()` solo enumera promovidos actuales.

**DECIDED:** después de cambios de asignación, se mantiene **un solo archivo de snapshot operacional consolidado**, sobrescrito con el estado vigente y **sin versionado propio**. Incluye exclusivamente usuarios con algún dato operacional asignado (al menos uno de `area_id`, `position_id` o `group_id` no nulo). Si un usuario deja de tener datos asignados, no debe aparecer en el siguiente snapshot. Su objetivo exclusivo es permitir la **recuperación conjunta** de esas asignaciones; aplicaciones y workers nunca lo consumen. No equivale al historial existente de los Source individuales.

**OPEN DE DISEÑO, sin reinterpretar la decisión anterior:** definir esquema y ruta física del único archivo, método de actualización atómica, control de concurrencia, recuperación de escrituras interrumpidas e inventario completo de asignaciones. La continuidad o retirada de los Source individuales actuales requiere decisión explícita de migración; no eliminarlos ni declarar que el snapshot ya está implementado. También falta determinar cómo afectan al inventario los usuarios retirados de la lista de promovidos.

**No confundir:** una nueva versión vigente de una proyección individual en Cosmos no equivale a un registro histórico append-only. Si «un registro por cambio en Cosmos» significa guardar eventos separados además del documento vigente, es un requisito distinto y actualmente OPEN/UNVERIFIED.

## Secuencia incremental

| Orden | Incremento | Estado | Entregable y aceptación mínima |
|---|---|---|---|
| 0 | Higiene focalizada | **OPEN / IN PROGRESS** | Resolver `I001` en `service.py` y mantener espejo comentado equivalente; ejecutar Ruff y suite completa en scope, sin ampliar alcance. |
| 1 | Completar contrato técnico del snapshot | **PLANNED / DESIGN** | Conservar el significado DECIDED (un archivo sobrescrito, sin versiones, solo usuarios con datos). Resolver esquema, inventario de usuarios, tratamiento de retirados, convivencia/migración de Sources actuales, disparador, concurrencia y recuperación. Sin código antes del acuerdo. |
| 2 | Backend snapshot operacional | **PLANNED / BLOCKED por 1** | Implementar mínimo contrato acordado, publicación/reconstrucción verificable, ETag o concurrencia equivalente, idempotencia/fault recovery y pruebas; producción + espejo comentado. |
| 3 | Manager UX de dos pestañas | **PLANNED / SEPARATE** | Reordenar pantallas según diseño, integrar estado/trazabilidad general y verificar callbacks; apariencia/responsive con revisión visual, no snapshots de CSS. |
| 4 | Proyección y recuperación integrada | **PLANNED / SEPARATE** | Verificar guardado Source → proyección Cosmos del catálogo y por usuario, reintentos, versiones exactas y no interferencia con otras familias documentales; pruebas Azurite/Cosmos cuando proceda. |
| 5 | Resolución de sesión ADA | **PLANNED / SEPARATE** | Sesión Entra/Users resuelta, `guest` antes de promoción, recarga tras promoción, consulta individual en Cosmos, unión con catálogo operativo; sin acceso a Blob desde runtime. |
| 6 | Warmup de catálogos | **PLANNED / SEPARATE** | Solo perfiles y catálogo operacional desde Cosmos; refresco periódico configurable, inicial propuesto de 10 minutos, retención de último dato válido y estados de degradación; cero precarga de usuarios/asignaciones. |

No convertir esta tabla en autorización para desarrollar simultáneamente todos los incrementos. En cada chat un solo incremento implementable.

## Riesgos y pruebas que NO deben falsearse

- Fallo después de publicar Source y antes de proyectar en Cosmos: persistir resultado durable y reintentar proyección, no suponer transacción única. Para nuevo snapshot, especificar también recuperación tras interrupción y orden entre publicaciones.
- Cambio de etiquetas del catálogo: las asignaciones mantienen IDs; no replicar etiquetas en cada documento individual de forma innecesaria.
- Cambio de promoción/enabled mientras existe una caché: una caché compartida de catálogos no reemplaza controles de sesión/autorización.
- Actualización de warmup en varios procesos: caché por proceso salvo contrato explícito de infraestructura compartida; no introducir Redis preventivamente.
- Recontar pruebas solo del scope y commit identificados. CI, Azure/Entra y visual no han sido validados por la prueba local de 25 casos.
- El objetivo transversal Python 3.14.7 y la metadata `==3.14.2` de este paquete continúan como incompatibilidad documental/técnica por tratar en incremento aislado.

## Próximo foco único

**Completar diseño técnico del snapshot único ya definido funcionalmente**, tras validar la corrección de higiene aislada (`I001`). No mezclar implementación de snapshot con Manager, sesión, worker ni warmup. Una vez aceptado el contrato, entregar solo archivos nuevos/modificados con tests de comportamiento y espejo pedagógico.
