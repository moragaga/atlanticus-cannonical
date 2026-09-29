# Atlanticus — Open Questions

Estado: **OPEN ITEMS POR FRENTE**. Actualización focal: Manager M01 CLOSED / ADA M02 PLANNED (2026-09-29). No recalificar otros ámbitos por una prueba local de Manager.

## KPI backend y Collector — checkpoints históricos CLOSED

```text
KPI-RUNTIME-REPROCESS-CURRENT                 CLOSED / checkpoint anterior
KPI-DELIVERY-REGISTRY-CONSUMPTION            CLOSED / checkpoint anterior
KPI-TIMESERIES-REGISTRY-CONSUMPTION          CLOSED / checkpoint anterior
KPI-HISTORIAN-REPROCESS-CURRENT              CLOSED / checkpoint anterior
ATLANTICUS-WEB-OBSERVABILITY-SERVICE         CLOSED / checkpoint anterior
ADA-WEB-KPI-COLLECTOR-CAPABILITY             CLOSED / checkpoint anterior
KPI-COLLECTOR-DEFINITION-ATTACHMENT          CLOSED / checkpoint anterior
KPI-COLLECTOR-REAL-WEB-SMOKE                 CLOSED / checkpoint anterior
```

Siguen cerrados según **sus** checkpoints: intervalos Latest/Timeseries, política de coherencia, stores, mapping, ciclo de poller, observability y attachment de `WebApplicationDefinition`.

## OPEN — integración operacional del Collector, frente KPI independiente

`ADA-GENERIC-COLLECTOR-OPERATIONAL-INTEGRATION` permanece **PLANNED / NEXT del frente KPI**. Ubicar en `atlanticus:main` la composición operacional real con ToolConfiguration, Tool projection revision y cliente/configuración Cosmos; montar `AdaKpiCollector` mediante el attachment existente, sin crear otra aplicación. La aceptación incluye ToolStructure real, health sin poller, primera petición que lo inicia y Latest/Timeseries disponibles en browser stores.

`KPI-INSPECTION-DEFINITION-PROVIDER-REALIGNMENT` permanece OPEN / SEPARATE. Optimizaciones `Latest Delivery REPROCESS_CURRENT`, `Timeseries Delivery REPROCESS_CURRENT` y `Historian reprocess_from` permanecen PROPOSED / DEFERRED.

## OPEN transversal, no ligado a M01

- `PYTHON-METADATA-ALIGNMENT`: baseline Project 3.14.7 frente a paquetes/uv Manager que todavía utilizan 3.14.2. OPEN / SEPARATE; no actualizar incidentalmente metadata/lockfiles durante M02.
- `FULL-BACKEND-PYTEST-TOPOLOGY`: revalidar fallos históricos de collection alrededor de `tests.support`; no asumir su presencia o ausencia actual. BLOCKED / SEPARATE.

## M01 Manager — cuestiones cerradas y límite de evidencia

**CLOSED / VERIFIED:** implementación opcional `ManagerCompanionView` publicada en `atlanticus:main@9cc2cebe595ef1341830374ad2bb3c61baf6f5a2` y pruebas locales comunicadas 82 PASS con mirrors AST. La publicación remota del commit fue comprobada posteriormente: **ya no** está pendiente de push. No abrir nueva implementación genérica por este cierre.

**OPEN / SEPARATE:** seis incidencias Ruff previas a M01 (source import; `ManagerError` sin uso en layout; test_brand_header F401; tests registry/surface/workspace I001). El gate global de Ruff **no** está limpio; la suite global/CI y el smoke visual de ADA M02 son UNVERIFIED. Corregir baseline requiere otro alcance, no M02.

## OPEN / BLOCKED DESIGN — M02 ADA Datos operacionales

1. **Conflicto documental de orden y labels.** Canonical anterior: «Datos operacionales» primero, «Asignación» después. Diseño posterior del chat: «Asignaciones» como companion inicial y «Catálogo de cargos» como módulo administrativo. **OPEN** hasta reconciliación explícita con decisiones vigentes; no modificar código antes del acuerdo.
2. **Conexión exacta del workspace del catálogo.** El código ya posee `OperationalCatalogDraftEditor`, Source/Validation/Projection contracts y servicios registrados condicionalmente. Confirmar su wiring para convertir `ManagerEntry` a `ManagerModule` sin duplicar servicios, inventar configuración ni conservar publicación directa legacy. PLANNED / DESIGN.
3. **Pruebas M02 todavía inexistentes.** Validar no interferencia entre catálogo y asignaciones, CAS/reintento, guardado de borrador sin publicación, selección de cargos activos/proyectados, 10/20 páginas y regresión Manager; presentación/modal/responsive por inspección visual. UNVERIFIED.
4. **Labels de proveedores.** La UI operacional actual recibe nombres de los providers de Tool en la composición. Confirmar fuente/metadatos de proveedor que realmente corresponde al dominio operacional, sin reemplazar por etiquetas inventadas. OPEN / DESIGN.

## OPEN — Snapshot operacional, sesión y warmup: frentes separados

- **DECIDED significado del snapshot, NOT IMPLEMENTED:** un solo archivo durable sobrescrito, sin versionado propio, con usuarios que tienen al menos un campo operacional asignado; sirve solo para reconstrucción colectiva y no para lectura runtime. OPEN técnico: schema/ruta, inventario completo incluso ex-promovidos, disparador, CAS/concurrencia, atomicidad, recovery e intención explícita sobre convivencia/migración de los Source individuales. Implementación BLOCKED por ese contrato; M02 no presupone dicho cambio.
- **UNVERIFIED:** un evento Cosmos append-only por cambio; el store inspeccionado mantiene un documento de proyección vigente por usuario y no demuestra histórico de eventos separado. Decidir solo ante un requisito real.
- **Sesión individual Cosmos:** PLANNED / UNVERIFIED wiring Entra. Usuario no promovido = guest; requiere recarga tras promoción; resolver atributos individuales bajo demanda desde Cosmos y catálogo común.
- **Warmup:** PLANNED / solo catálogo Profiles y catálogo operacional. Excluye usuarios, promociones, asignaciones y snapshot. Intervalo de 10 min PROPOSED, no runtime implementado.
- **Lint operacional histórico:** `I001` en `operational-identification/service.py` fue reportado en otro hito; su persistencia en HEAD actual es **UNVERIFIED**, revalidar si se abre ese frente. No confundirlo con las seis incidencias Manager de M01.
- **UNVERIFIED:** Azure/Entra E2E, Docker/integración física, varios workers y ejecución clean-checkout contra el nuevo M01.

Referencia de frontera: `10_MANAGER/11_ADA_OPERATIONAL_DATA_ROADMAP.md`. El NEXT M02 corresponde exclusivamente al siguiente chat del frente Manager y no sustituye el NEXT propio del KPI Collector.
