# Alarm Engine — Index

Estado documental propuesto: **CURRENT / B2a y B2b IMPLEMENTADOS EN MAIN, VALIDADOS LOCALMENTE / B2c PLANNED**.

Corte de lectura (2026-09-27):

```text
Implementación: moragaga/atlanticus:main           ebc7a8bf8d49e931fd4e2487dac5ee036011a0a5
Canonical consultado: moragaga/atlanticus-cannonical:main be2c424c44648e6488daae36d410cf425eed02b8
Decisions consultado: moragaga/atlanticus-decisions:main 50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

Estos documentos son **reemplazos locales preparados para revisión e integración humana**. No acreditan que `atlanticus-cannonical:main` ya se haya actualizado. El HEAD de implementación contiene también commits de otros frentes; el cierre documental aquí sólo abarca Alarm Engine B2a/B2b.

| Archivo | Responsabilidad | Situación de este corte |
|---|---|---|
| `01_DOMAIN_MODEL.md` | Identidad, Core y contratos de dominio | CURRENT; no se reemplaza. |
| `02_RUNTIME_AND_LIFECYCLE.md` | Reductor y frontera entre lifecycle y adopción | CURRENT; reemplazo puntual de sección de adopción, sin redefinir Core. |
| `03_PERSISTENCE_AND_RECOVERY.md` | WAL, V1/V2, Durable/Materialized y EFFECTIVE | CURRENT; reemplazo necesario. |
| `04_CONCURRENCY_LEASES_AND_FENCING.md` | Leases, autoridad y fencing | CURRENT; no se reemplaza. |
| `05_PROJECTION_AND_PUBLICATION.md` | READY, EFFECTIVE y futuras proyecciones | CURRENT; reemplazo necesario. |
| `06_MANAGEMENT.md` | Acciones, effects y deactivation | CURRENT; no se reemplaza. |
| `07_CONFIGURATION_AND_MATERIALIZATION.md` | Source v3, B.2, lector y planner B1 | CURRENT; reemplazo de su frontera con B2a/B2b. |
| `08_QUALIFICATION_BASELINE.md` | Evidencia histórica y gates locales | CURRENT; incorporar los cuatro gates, sin reinterpretar F-010. |
| `09_DECISION_INDEX.md` | Decisiones, refinamientos, discrepancias | CURRENT; actualizar clasificación. |
| `10_OPEN_ITEMS.md` | OPEN concretos y próxima frontera | CURRENT; quitar falsos pendientes B2a/B2b. |
| `11_SOURCE_LEDGER.md` | Trazabilidad, SHAs, pruebas y límites | CURRENT; ampliar evidencia. |
| `12_COMMAND_CENTER_ANALYTICS_BOUNDARY.md` | Frontera conceptual Analytics | CANDIDATE, debate separado; no se reemplaza. |
| `13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md` | Contratos exactos y brecha del ejecutor | CURRENT para B2a/B2b; B2c PLANNED. |

## Cadena real de configuración en este corte

```text
Source v3: Rn + ToolDependencyManifest(Cn)
  -> proyección de entrada adquirida por Materialization mediante Cosmos
  -> resolver B.2 + qualification -> READY o BLOCKED
  -> publicación local de pareja Runtime/Delivery exacta e inmutable
  -> READY sigue siendo sólo candidata, no autoridad de ejecución
  -> B1: AlarmConfigurationArtifactRef + Revision + AdoptionPlan
  -> B2a: WAL global V1 (0 grupos) o V2 (1..N grupos)
  -> B2b.1: Effective Head recuperable y validado desde WAL/snapshots
  -> B2b.2: Runtime puede cargar la revisión EFFECTIVE exacta
  -> [PLANNED B2c] ejecutar todas las disposiciones admitidas y vincular el ejecutor a V1/V2/EFFECTIVE
  -> [SEPARATE] fuentes/evaluadores operativos, Live Delivery, Management Capture, History
```

**READY != EFFECTIVE**. B2b.2 implementa una capacidad explícita de lectura, **no** la conexión automática del job existente con dicha lectura ni la ejecución completa de adopciones. La elección de la versión efectiva nunca se deriva de latest READY. La proyección `effective-head.json` no es otra autoridad: se reconstruye del WAL.

**Próximo foco único recomendado:** B2c, debate contractual y ejecución segura del planificador B1 contra el ejecutor existente, antes de integrar commits V1/V2. No abrir fuentes, Delivery ni History en ese mismo incremento.
