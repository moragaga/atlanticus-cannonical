# Atlanticus — Authority

Estado: **CURRENT — actualización documental acotada a ADA Command Center / Alarm Engine B2c.7, 2026-09-28**. Este corte **no recalifica** otros dominios de Atlanticus ni sustituye sus documentos canónicos especializados.

## Fuentes y límites del corte

| Repositorio | Referencia | Qué respalda |
|---|---|---|
| `moragaga/atlanticus` | `c67fcb5b105cc561c16719a8bca4ea5aa74c3fae` | Commit B2c.7d confirmado por el usuario **y verificado independientemente en Git**: ocho archivos del incremento; contratos FACTS v2, productor, receptor y pruebas inspeccionados. |
| `moragaga/atlanticus:main` | `bc1d73742bcb04eb495bbbb1725a8ad23d4eff38` | HEAD remoto leído al cierre, un commit después de c67fcb5. El único commit posterior modifica ADA Generic Master Projection, no Alarm Engine ni Delivery. |
| `moragaga/atlanticus-decisions:main` | `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e` | Decisiones documentadas, intención contractual e historia; no implica implementación automática. |
| `moragaga/atlanticus-cannonical:main` | `5558cf9d92d9b21758500024b6099011416d78da` | Documentación real leída antes de preparar los reemplazos de este cierre. |

**No confundir** un commit local notificado con un commit remoto independientemente verificado. Antes de integrar los reemplazos, comprobar que las ramas y archivos de destino no hayan cambiado.

## Jerarquía de autoridad y resolución de conflictos

1. `atlanticus:main`: realidad implementada actual, con SHA inspeccionable.
2. Decisiones explícitamente vigentes y congeladas en `atlanticus-decisions:main` y contratos del Project: intención contractual; si contradicen el código, registrar `CONFLICT`, no escoger o modificar en silencio.
3. Archivos canónicos vigentes del Project y `atlanticus-cannonical:main`: descripción documental del estado, que debe contrastarse con código y decisiones; los reemplazos locales candidatos no son versiones ya incorporadas.
4. Qualification, tests y logs ligados al commit/árbol y entorno: evidencia limitada a escenarios realmente ejecutados.
5. Historial conversacional: pista de búsqueda, nunca autoridad suficiente.

Cuando código, decisions y canonical difieran, documentar `CONFLICT` y mantener por separado `VERIFIED`, `INFERRED`, `ASSUMED`, `PROPOSED`, `UNVERIFIED` y `CURRENT`, `IN PROGRESS`, `PLANNED`, `SUPERSEDED`, `BLOCKED`, `CLOSED`.

Git es **SOLO LECTURA** por defecto: no efectuar commits, push, nuevas ramas, PR, issues ni otras mutaciones sin autorización explícita. Repositorios ajenos son referencias sólo cuando el usuario los identifique.

## Frontera de Alarm Engine al corte B2c.7

**CURRENT según inspección remota B2c.7a/b, artefactos B2c.7c/d y logs locales:** Source v3; resolver B.2; READY/BLOCKED y pareja Runtime/Delivery exacta local; WAL con adopción V1/V2 y EFFECTIVE derivado; ejecución con sesión fijada y composición operacional; Engine publica CURRENT completo v1 y lotes FACTS inmutables con cursor de exportación. Delivery es un job independiente que recibe CURRENT y FACTS mediante archivos del volumen compartido, con cursor propio. B2c.7c verificó el recorrido Engine → Delivery con datos controlados y reinicio de componentes; B2c.7d introdujo FACTS v2 con referencia criptográfica al lote anterior y rechazo fail-closed de cadenas incompletas.

**VERIFIED por logs locales de usuario, no por CI remoto:** gate final B2c.7d, 32 pruebas específicas PASS; regresión conjunta Engine + Delivery, 162 PASS y 1 SKIPPED; Ruff lint PASS; Ruff format PASS para 61 archivos. El commit final `c67fcb5b105cc561c16719a8bca4ea5aa74c3fae` fue corroborado en Git junto con sus ocho archivos; **no** se reejecutó CI ni checkout limpio en este cierre.

**UNVERIFIED:** build/distribución del commit final, repetición de gates en checkout limpio, Docker de ambos procesos independientes, almacenamiento físico/multi-host, migración de cursores/volúmenes FACTS v1 existentes, destino real de publicación y CI.

## Invariantes conservados

```text
READY != EFFECTIVE
LATEST SAVED = LATEST VALID_AT_SAVE
VALID_AT_SAVE != READY != EFFECTIVE
INVALID != REMOVED
DISABLED != INVALID
DISABLED != REMOVED
TRACE_ONLY != REMOVED
AlarmResolutionKey = (alarm_configuration_revision, confirmed_tool_catalog_revision)
Exact artifact pin = (source_key, result_id, manifest_sha256, resolution_key)
WAL -> DURABLE HEAD -> SNAPSHOTS -> MATERIALIZED HEAD -> EFFECTIVE projection
Engine CURRENT = snapshot completo reemplazable v1, no WAL
Engine FACTS = batches confirmados e inmutables v2 con previous_batch
Delivery receipt cursor != Engine export cursor
Delivery no lee el WAL para consumo operacional; Web tampoco
```

El resolver B.2 puro no convierte al paquete Materialization completo en una dependencia sin I/O. Mantener Source v3 y manifest Tool exacto y strict routing PROCESS → INTEGRATED_OPERATIONS → STRATEGIC → END. No añadir compatibilidad legacy de FACTS v1 por conveniencia ni borrar volúmenes históricos: la transición v1→v2 es **BLOCKED** para un despliegue sobre datos existentes hasta inventario y decisión explícita. Paquetes Command Center inspeccionados exigen Python `==3.14.2`; objetivo transversal del Project: `3.14.7`. No alinear incidentalmente.

**Siguiente foco recomendado:** qualification de distribución y ejecución independiente Engine/Delivery en Docker, comenzando por validar artefactos y contrato de volumen en un entorno limpio. No mezclar Live Projection, Web, Capture ni Analytics.
