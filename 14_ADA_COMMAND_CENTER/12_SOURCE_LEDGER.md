# ADA Command Center — Source Ledger

Estado: **AUDIT LEDGER / UPDATED 2026-09-21**

## `atlanticus:main`

Checkpoint CURRENT de cierre:

```text
4fe03660ad47d105c55167dc583f09be1f395275
```

Parent inmediato:

```text
8e133ad7da3524874add8323315e8c2b3c3f1ee1
```

Tree:

```text
d893128c23939e8a1e8bdd60b5bae64d14983be8
```

### Commits relevantes acumulados

```text
8e5f7312eb7522a2345b1d225faa80c6eac6ec41
→ Alarm Configuration contract + Source/Release

07eeb8d4ecc3f1e9d9a84ab1059eaad2fd5f78ce
→ Alarm Configuration base Projection

1c67212b21ef2241bcb59173ccb8e9cd237a0219
→ Manager/workspace/workflows + capability-local Web surface

8e133ad7da3524874add8323315e8c2b3c3f1ee1
→ Command Center Tool Catalog V1

4fe03660ad47d105c55167dc583f09be1f395275
→ Alarm Tool Reference read model V1 inside Alarm Configuration
```

Inspeccionado/relevante:

- `scopes/ada-command-center/backend/tools/catalog`;
- `scopes/ada-command-center/web/alarms/configuration`;
- `scopes/ada-command-center/backend/alarms/core`;
- `scopes/ada-command-center/backend/processes/alarms-runtime`;
- `scopes/ada/web/tools/core`;
- `scopes/ada/web/tools/configuration`;
- `scopes/ada/web/tools/projection-cosmos`;
- `connectivity/storage`;
- `web/capabilities/projection`;
- `web/capabilities/source`;
- `web/capabilities/manager`.

## Qualification observada

### Tool Catalog V1

Checkout real del usuario:

```text
pytest                                 13 passed
ruff check .                           All checks passed!
ruff format --check .                  13 files already formatted
```

### Alarm Tool References V1

Checkout real del usuario:

```text
pytest                                 32 passed
ruff check .                           All checks passed!
ruff format --check .                  1 test required reformat
```

Se indicó formatear `tests/test_tool_references.py` antes del cierre y después se publicó
`main@4fe03660...`.

No se recibió salida explícita de una corrida post-publicación de los tres gates sobre ese SHA.

Clasificación:

```text
functional qualification before final format    VERIFIED
lint before final format                        VERIFIED
exact post-publication full qualification       UNVERIFIED
```

## Tool Catalog CURRENT

Existe físicamente:

```text
scopes/ada-command-center/backend/tools/catalog
```

Implementa:

- `ToolCatalogEntry`;
- `ToolCatalogSnapshot`;
- deterministic SHA-256 revision;
- `ToolCatalogStore`;
- `ToolCatalogInput`;
- `ToolCatalogConsolidator`;
- `BlobToolCatalogStore`;
- JSON codec schema version 1;
- commented pedagogical mirror;
- unit tests.

Document type:

```text
ada_command_center_tool_catalog
```

Schema version:

```text
1
```

No implementa Cosmos de Command Center, history, scheduler ni availability states por entry.

## Alarm Tool References CURRENT

Existe físicamente:

```text
scopes/ada-command-center/web/alarms/configuration/
src/ada_command_center/web/alarms/configuration/tool_references.py
```

Implementa:

- `AlarmToolSubcomponentReference`;
- `AlarmToolComponentReference`;
- `AlarmToolReference`;
- `AlarmToolReferenceCatalog`;
- `AlarmToolReferenceReader`.

Alarm Configuration ahora depende explícitamente de:

```text
ada-command-center-tools-catalog==0.1.0
ada-web-tools==0.1.0
```

No se modificaron los contratos durables de `AlarmConfiguration`.

## `atlanticus-cannonical`

Checkpoint inspeccionado antes de este reemplazo:

```text
fb5b000d0f38a535fca606fe01a324cb0f6185b4
```

Quedó desactualizado respecto de implementación al indicar Tool Catalog como NEXT/NOT IMPLEMENTED y
al mantener como siguiente prerequisite el contrato Tool Catalog antes de B.2.

## `atlanticus-decisions`

Uso: **HISTORICAL ONLY**.

Checkpoint observado:

```text
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

### B.2

Preserva decisiones útiles sobre:

- Live vs Management Projection;
- Runtime/Delivery desde una resolución común;
- LKG;
- `INVALID != REMOVED`;
- Web no resuelve prioridad ni configuración operacional.

Conflictos/refinamientos respecto de CURRENT:

- SharePoint como autoridad física queda superseded en dominios migrados;
- Tool/evaluator no bloquean persistencia intrínseca sólo por estar aún no resolubles;
- V1 no implementa Confirmed Tool Catalog + reconciliation states AVAILABLE/STALE/MISSING;
- Tool Catalog CURRENT usa Blob snapshot derivado y all-or-nothing refresh;
- B.2 sigue sin implementación física.

## Sin cambio por este hito

No se modifican conceptualmente:

- `04_ALARM_ENGINE/01_DOMAIN_MODEL.md`;
- `04_ALARM_ENGINE/12_COMMAND_CENTER_ANALYTICS_BOUNDARY.md`.
