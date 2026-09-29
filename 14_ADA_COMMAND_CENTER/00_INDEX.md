# ADA Command Center — Canonical Index

Estado: **CURRENT — C1 ownership Web CLOSED; C2 identidad operacional / Source Key / topología Cosmos CLOSED en código e integración local declarada (2026-09-29); C3/C4/C5 PLANNED; Starter, Live, History, Docker/Azure y aceptación visual independientes**.

## Autoridad y alcance del corte

- Implementación verificada por lectura directa de Git: `moragaga/atlanticus:main@18029e19ff01e58b9c9399c132ff32b5ca913f06`.
- Decisiones revisadas: `moragaga/atlanticus-decisions:main@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`; sus decisiones históricas pueden conservar topologías reemplazadas por implementación y dirección canónica posteriores.
- Base canónica que sustituyen estos documentos: `moragaga/atlanticus-cannonical:main@15ba51fb5a140601fd4e2a8d78a01a5b87c6eeaa`.
- Evidencia local entregada por el usuario: `domain/alarms` 56 PASS; tres procesos backend 238 PASS, 1 SKIPPED y 1 test del registro operacional excluido durante el gate; Configuration Manager 28 PASS; prueba específica de configuración Materialization 1 PASS; `uv lock --check` satisfactorio. El usuario corrigió después el test de registro operacional y el commit actual contiene la corrección. **UNVERIFIED:** nueva ejecución total del backend sin exclusiones posterior a esa corrección, CI, Docker y recursos Azure físicos.
- C1 permanece CLOSED según su evidencia histórica; sus pruebas no se reasignan a C2. C2 CLOSED estructuralmente y con gates locales acotados; no se declara qualification operativa física.

## Navegación documental

| Documento | Responsabilidad y estado después de C2 |
|---|---|
| `01_PRODUCT_SCOPE.md` | Alcance de producto, no modificado por C2. |
| `02_CURRENT_IMPLEMENTATION.md` | Implementación vigente de C1/C2 y límites de prueba. **REEMPLAZO C2**. |
| `03_WEB_APPLICATION.md` | Host temporal y composición Web; Source Key consumida desde Domain. **REEMPLAZO C2**. |
| `04_CONFIGURATION_SCOPE.md` | Alarm Source v3 y contratos de identidad/configuración ya normalizados. **REEMPLAZO C2**. |
| `05_TOOL_TO_ALARM_CONFIGURATION.md` | Tool manifest Rn/Cn y routing, preservados; sin modificación C2. |
| `06_ENGINE_AND_PROJECTIONS.md` | Engine, Materialization y Delivery actuales con identidad compartida; Live separado. **REEMPLAZO C2**. |
| `07_ANALYTICS_AND_STORYTELLING.md` | Analytics independiente; sin modificación C2. |
| `08_INITIAL_DASHBOARD.md` | Dashboard conceptual; sin modificación C2. |
| `09_IDENTITY_NAVIGATION_PROFILES.md` | Identidad, navegación y perfiles; sin modificación C2. |
| `10_INITIAL_OUT_OF_SCOPE.md` | No objetivos; sin modificación C2. |
| `11_GOLDEN_PATH.md` | Ruta end-to-end, C2 completado y gates aún abiertos. **REEMPLAZO C2**. |
| `12_SOURCE_LEDGER.md` | Histórico inmutable C1/B1d/B2c y nuevo checkpoint C2. **REEMPLAZO C2**. |
| `13_OPEN_ITEMS.md` | C2 cerrado; C3/C4/C5 y frentes independientes abiertos. **REEMPLAZO C2**. |
| `14_TOOL_CATALOG.md` | Owner Web C1; sin cambio C2. |
| `15_ALARM_CONFIGURATION_AUTHORING_MODEL.md` | Authoring/UX y Source v3, sin refactor C2. |
| `16_ALARM_LIVE_DELIVERY_CONTRACT.md` | CURRENT receiver CURRENT+FACTS; futuro C4 CURRENT-only y Live distinto. Sin cambio contractual C2. |
| `17_DOMAIN_OWNERSHIP_AND_MIGRATION.md` | Domain identidad transversal y dependencias que siguen abiertas. **REEMPLAZO C2**. |
| `18_ALARM_AUTHORING_UX_AND_VISUAL_PRESENTATION.md` | UX pendiente e independiente; sin modificación C2. |

## Fronteras siguientes, no iniciadas por este cierre

- **C3 PLANNED / BLOCKED BY DESIGN:** qualification verificable, productor y evaluadores GREEN reales aún no acreditados; archivo manual actual sigue válido.
- **C4 PLANNED / FOCO RECOMENDADO:** cambiar receptor de Delivery de CURRENT+FACTS al último CURRENT; mantener Engine FACTS v2 y WAL. Requiere auditoría/diseño antes de código.
- **C5 PLANNED:** identificación del contrato técnico de evidencia y revisión restante de `.env.detail`; no inventar key/version.
- **SEPARATE:** prueba Docker de jobs independientes, recursos físicos Blob/Cosmos, distribución aislada, Starter Web, Live Delivery, Management Capture, History/Analytics y ajustes UX.

**Regla de cierre:** Git continúa SOLO LECTURA durante la documentación. Este paquete es reemplazo propuesto de los archivos listados, no un commit ni evidencia de nuevas pruebas.
