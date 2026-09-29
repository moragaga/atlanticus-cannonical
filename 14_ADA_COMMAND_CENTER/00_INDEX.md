# ADA Command Center — Canonical Index

Estado: **CURRENT TRACKING — C1/C2/C4 CURRENT/CLOSED según sus gates; UX-01/UX-02 CLOSED en editor y aceptación funcional local básica; Starter propio con runtime/Home PLANNED; Golden Path completo y aceptación productiva UNVERIFIED**. Reemplazo UX/Starter preparado: 2026-09-29.

## 1. Autoridad y alcance del corte

```text
Implementation inspeccionada   moragaga/atlanticus:main@2e7500a6b8b4d5bbdad26d807abfa57936db99d5
Decisions inspeccionadas      moragaga/atlanticus-decisions:main@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical remoto previo       moragaga/atlanticus-cannonical:main@2e8bbf4780cafc4cea3b18351861aa97a4fb0053
```

La autoridad de implementación es `atlanticus:main`; las decisiones frozen siguen siendo intención contractual y sus discrepancias no se corrigen silenciosamente. Canonical aún **no** refleja completamente C4 ni UX-01/UX-02. Reconsultar HEAD y comparar diff antes de integrar: existieron **11 reemplazos C4 preparados en otro cierre**, pero su integración Git es **UNVERIFIED**. Este paquete UX sólo sustituye **seis** páginas de alcance, sin alterar el detalle histórico de páginas C4 restantes. Git SOLO LECTURA.

## 2. Navegación documental

| Documento | Propósito / estado conocido |
|---|---|
| `01_PRODUCT_SCOPE.md` | Producto independiente, transversal y distinto de ADA Generic. Sin modificación en este paquete. |
| `02_CURRENT_IMPLEMENTATION.md` | Componentes reales C1/C2/C4; **reconciliar con reemplazo C4 anterior**, no sobrescribir con baseline pre-C4. |
| `03_WEB_APPLICATION.md` | Host temporal, local aislado de prueba, UX y próximo Starter genérico con Home mínima. **REEMPLAZO UX/STARTER DE ESTE PAQUETE**. |
| `04_CONFIGURATION_SCOPE.md` | Source v3, manifest Cn, contratos estáticos actuales. **Reconciliar C4 anterior + UX-02**; no incluido para no perder trabajo histórico. |
| `05_TOOL_TO_ALARM_CONFIGURATION.md` | Tool Manifest exacto/Rn-Cn y routing. Preservado, no modificado. |
| `06_ENGINE_AND_PROJECTIONS.md` | Materialization, Engine y C4 receptor CURRENT-only. **Reconciliar reemplazo C4 anterior**, sin gate físico nuevo en UX. |
| `07_ANALYTICS_AND_STORYTELLING.md` | Analytics independiente. No modificado. |
| `08_INITIAL_DASHBOARD.md` | Dashboard conceptual futuro, fuera de Starter inicial. No modificado. |
| `09_IDENTITY_NAVIGATION_PROFILES.md` | Identidad, navegación y perfiles fuera del primer Starter. No modificado. |
| `10_INITIAL_OUT_OF_SCOPE.md` | No objetivos de producto, preservar sin añadir features por inferencia. |
| `11_GOLDEN_PATH.md` | Gate end-to-end real frente a pruebas parciales, C4 y próximo Starter. **REEMPLAZO UX/STARTER DE ESTE PAQUETE**. |
| `12_SOURCE_LEDGER.md` | Historia de implementación/qualification; **reconciliar C4 anterior y anexar evidencia UX**. No se entrega reemplazo unilateral del ledger histórico. |
| `13_OPEN_ITEMS.md` | Estados consolidados, findings UX, discrepancia Decisions/Git, siguiente frontera. **REEMPLAZO UX/STARTER DE ESTE PAQUETE**. |
| `14_TOOL_CATALOG.md` | Owner Web C1 y catálogo durable; sin modificación de contrato. |
| `15_ALARM_CONFIGURATION_AUTHORING_MODEL.md` | Source v3, UX-01/UX-02 y conflicto B.1 `1..12` vs Git `1..11|END_OF_SHIFT`. **REEMPLAZO UX DE ESTE PAQUETE**. |
| `16_ALARM_LIVE_DELIVERY_CONTRACT.md` | Contrato conceptual de Live; **reconciliar C4 anterior**, NO confundir receiver CURRENT-only con Live implementado. |
| `17_DOMAIN_OWNERSHIP_AND_MIGRATION.md` | Domain/Tool/Web/Backend ownership; **reconciliar C4 anterior**. No refactor en este cierre. |
| `18_ALARM_AUTHORING_UX_AND_VISUAL_PRESENTATION.md` | UX aceptada básica, defectos OPEN, visuales futuros intactos. **REEMPLAZO UX/STARTER DE ESTE PAQUETE**. |

Los documentos raíz `00_AUTHORITY.md` y `01_CURRENT_STATE.md` incluyen baselines anteriores de otros frentes del Project. Su actualización es un **refresh global separado**, no una sustitución implícita con este paquete. Decisions conserva B.1 DESIGN FROZEN hasta revisión humana de la discrepancia UX-02.

## 3. Corte UX — evidencia VERIFIED y alcance CLOSED

- UX-01 (Save Draft de modal): cierre tras éxito, no tras error; test Web anterior **122 PASS** reportado por el usuario.
- UX-02 (Rule default y Message override): `1..11` horas o `END_OF_SHIFT`; Domain, codec, Materialization y UI modificados en 20 archivos; Git commit remoto `2e7500a...` **VERIFIED**.
- Regresión UX-02 reportada por el usuario: Domain **59 PASS**, Materialization **64 PASS**, Configuration Web **123 PASS**, total **246 PASS**; `git diff --check` limpio.
- Prueba local aislada sin Azurite/Cosmos con host real y UNA Tool Process de fixture en catálogo archivo: usuario confirmó carga y pudo crear familias, alarmas y mensajes, asignarlos y guardar. **CLOSED** exclusivamente como aceptación básica de esas operaciones.

**OPEN observados:** pérdida de algunos valores, validaciones que desaparecen y alertas pegadas. Aceptación visual exhaustiva, reinicio/recovery y `END_OF_SHIFT` operacional **UNVERIFIED**. La opción estática no aporta `effective_until` UTC real por sí sola.

## 4. Baseline operacional ajeno al hito UX

C1 Web Tool ownership y C2 identidad/Source Key están cerrados bajo sus gates previos. C4 Delivery CURRENT-only está implementado en Git: recibe el último CURRENT validado, sin consumir FACTS; Runtime mantiene FACTS v2 para trazabilidad futura. Materialization READY/Engine EFFECTIVE exactos existen con qualification manual controlada. Esto **no** certifica infraestructura real, Docker independiente, enlace físico Blob/Cosmos ni `AlarmLiveProjection`.

**Conflicto explícito Decisions:** B.1 frozen sección 11 describe `max_duration_hours` entero `1..12` y shift end pendiente; Git UX-02 implementa `1..11 | END_OF_SHIFT` bajo el mismo campo y conserva Source v3. La resolución formal/versionado y consumidor operacional son OPEN. No agregar legacy ni cambiar código sólo para normalizar documentos.

## 5. Frontera exclusiva siguiente — PLANNED

**Diseño del Starter genérico propio de ADA Command Center con runtime y página de inicio mínima**, orientado a demostrar un flujo reproducible que ejecute alarmas con contratos existentes. Sin Navigation, Users, Profiles, dashboard complejo, History/Analytics ni nuevo frente UX. Auditar primero implementación y decisiones: composición Web/host, distribución, providers, Tool Catalog real, Alarm Source/Projection, qualification manual permitida, READY, Runtime CURRENT y Delivery CURRENT-only. Si el gate exige ver alarmas operacionales resueltas en Home, documentar que Live materializer/AlarmLiveProjection está **NO IMPLEMENTED**; diseñar contrato antes de consumidor, no simularlo.

**Límites:** contratos antes que consumidores; backend antes de frontend; no paquetes monolíticos, shims, datos inventados, tests CSS o cambios remotos sin autorización explícita.
