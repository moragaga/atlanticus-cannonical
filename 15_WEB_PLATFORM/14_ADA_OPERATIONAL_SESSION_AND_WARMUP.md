# ADA Web — Sesión, consumo operacional y warmup de catálogos

Estado: **DECIDED DESIGN / PLANNED INTEGRATION**, 2026-09-29. Documento especializado de consumo ADA. No atribuir implementación de estos nuevos flujos al bootstrap actual.

## Autoridad

Código verificado en `moragaga/atlanticus:main@caced5d7711cf059d36ec61aecc9b3e9629bd41f`:
- `OperationalIdentificationService.assignment_for_resolved_user` y `catalog_for_read`: APIs disponibles para consumo de proyecciones.
- `OperationalAssignment`: IDs y campos opcionales; el catálogo operacional contiene etiquetas compartidas.
- `ProfileCatalog` y `ProfileDefinition`: catálogo con `key`, `label`, `background_color`, `text_color`.
- `UserRecord`: identidad, `profile_key`, `enabled` y colores opcionales propios del usuario, separados del catálogo Profile.
- ADA Generic bootstrap actual: integración de identidad/Manager y otras capacidades, pero **no verificar aquí un warmup operacional o el wiring integral de asignaciones con sesión Entra**.

## Frontera durable/runtime — DECIDED

```text
MANAGER / ADMINISTRATION / RECOVERY
  Blob Source + historial -> proyección validada en Cosmos

RUNTIME ADA WEB / WORKERS
  Cosmos Projection -> resolución / cache opcional -> consumo
  Blob Storage: NUNCA fuente de lectura operacional del runtime
```

`users-runtime` y `users-support` poseen responsabilidades distintas. Promoción/estado del usuario pertenecen a Users; catálogo de Profiles y documentos operacionales pueden estar en `users-support` según wiring físico. La composición debe inyectar clientes y contenedores concretos, no inventar conexiones globales ni suponer la misma configuración de Cosmos para todos los dominios.

## Resolución de sesión — DECIDED / PLANNED

1. Autenticación Entra según la composición real; resolver identidad/promoción de **ese usuario**, no consultar masivamente todos los usuarios mediante warmup.
2. Sin promoción: identidad transitoria `guest`; no interpretar `guest` como perfil administrado final.
3. Promoción administrativa: los permisos y el perfil de una sesión previamente abierta no se actualizan automáticamente; es obligatorio **recargar la página** para re-resolver. La recarga funcional no sustituye un mecanismo server-side de revocación ante cambios sensibles de autorización.
4. Para usuario promovido, habilitado y autorizado, consultar su proyección **individual** en Cosmos. Unir IDs de área, cargo y grupo con catálogo operacional vigente, disponible como proyección compartida.
5. Si faltan catálogo/asignación/proyección, devolver estado funcional explícito o datos opcionales nulos según contrato; no completar información inventada ni recurrir a Blob.

El backend implementado ya rechaza `guest`/deshabilitado en `assignment_for_resolved_user` y trata la identidad local por separado; conectar ese contrato al ciclo completo de sesión requiere su propio incremento y pruebas.

## Warmup — DECIDED BOUNDARY / IMPLEMENTATION PLANNED

**Solo las siguientes proyecciones compartidas entran en warmup:**

| Documento | Información | Origen |
|---|---|---|
| Catálogo Profiles | Claves, nombres y colores de perfiles | Cosmos, proyección Profiles activa |
| Catálogo operacional | Cargos (`id`, `label`, `active`), referencias Mina/Planta, grupos 1–4 | Cosmos, proyección operacional activa |

**Prohibido dentro de este warmup:** usuarios promovidos, candidatos, identidades `guest`, asignaciones por usuario, colores individuales de `UserRecord`, snapshot consolidado de usuarios y cualquier listado completo de `users-runtime`.

- **PROPOSED:** carga inicial y refresco periódico configurable, con **10 minutos como default inicial sujeto a pruebas**. El usuario ha considerado 5–10 minutos; el intervalo exacto, modo de activación y tiempos de espera siguen PLANNED, no codificados.
- Cada proceso mantiene su propia copia válida si la necesita. No presumir memoria compartida entre Web y workers; no introducir Redis salvo razón y contrato independientes.
- El refresco no debe reemplazar una copia válida por un resultado parcial/corrupto. Registrar versión de proyección y estado de error. Definir explícitamente política de invalidez/desactualización antes de usar el catálogo para operaciones sensibles.
- Datos descriptivos pueden aprovechar la caché; decisiones de autorización mantienen la semántica actual de Users/Profiles/Access y no dependen exclusivamente del refresco periódico.
- La carga de asignación de un usuario es **bajo demanda durante su sesión**; el warmup nunca intenta amortizarla mediante precarga masiva de usuarios.

## Contratos que NO se modifican en este frente

```text
UserRecord no recibe area_id, position_id, group_id.
ProfileCatalog no almacena asignaciones de usuarios.
La pertenencia a un cargo no implica permisos ADA ni privilegios Manager.
El catálogo de cargos no es una fuente alternativa de identidad.
Un documento de proyección Cosmos vigente no equivale a historial append-only.
Blob es durable e histórico; el runtime consume Cosmos exclusivamente.
```

## Gates de aceptación futuros

- Pruebas de sesión `guest` → promoción → recarga → perfil/acceso e información operacional efectiva, sin refresco implícito de una sesión abierta.
- Pruebas de lectura individual Cosmos y resolución mediante catálogo, referencias faltantes y cambios de etiqueta sin reescribir todas las asignaciones.
- Pruebas de warmup que demuestren carga **solo** de los dos catálogos, refresco, reintentos y retención del último conjunto íntegro en fallos.
- Composición de aplicación y workers por sus propios settings/clients, sin lectura de Blob desde consumo. E2E Entra/Azure y multi-worker permanecen UNVERIFIED hasta ejecución real.

Consultar `../10_MANAGER/10_ADA_OPERATIONAL_IDENTIFICATION_BOUNDARY.md` y `../10_MANAGER/11_ADA_OPERATIONAL_DATA_ROADMAP.md`.
