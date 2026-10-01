# ADA Command Center — Current Implementation

Estado: **CURRENT — Source v3/Materialization/Runtime, C1 ownership Web, C2 identity and C4 Delivery CURRENT-only preserved; Manager 0.3.19 convergence CLOSED locally; final Generic Web application still NOT IMPLEMENTED.**

## Authority checkpoints

```text
Last confirmed Atlanticus HEAD in this chat:
36361dd570f86e8350ea4a6ee0e09bab351ba171

Command Center Manager convergence delta:
VERIFIED LOCAL / PENDING FINAL GIT HEAD
```

Historical checkpoints for C1/C2/C4 retain their own evidence and are not rewritten by this Manager close.

## Componentes existentes

```text
scopes/ada-command-center/
  domain/alarms/
  domain/tools/
  backend/alarms/core/
  backend/alarms/materialization/
  backend/alarms/persistence/
  backend/alarms/contracts/
  backend/processes/alarms-materialization/
  backend/processes/alarms-runtime/
  backend/processes/alarms-delivery/
  web/alarms/configuration/
  web/alarms/persistence/
  web/alarms/projection-local/
  web/alarms/projection-cosmos/
  web/tools/catalog/
  web/tools/discovery-cosmos/
  web/tools/catalog-manager/
  web/application/ada-command-center-configuration-manager/
```

Not implemented:

```text
ada-command-center-generic-application
final integrated Command Center Web composition root
```

`backend/tools` está SUPERSEDED. Domain Tools conserva contrato transversal; los servicios usados exclusivamente por Web viven en Web.

Materialization sigue importando `web/alarms/projection-cosmos`: frontera técnica OPEN heredada y no modificada en este hito.

## Manager convergence CURRENT locally

Single Manager authority:

```text
atlanticus-web-manager==0.3.19
```

Command Center packages aligned locally:

```text
ada-command-center-web-alarm-configuration==0.1.1
ada-command-center-web-tool-catalog-manager==0.1.1
ada-command-center-configuration-manager==0.1.1
```

Qualification:

```text
Alarm Configuration       124 PASS
Tool Catalog Manager        9 PASS
Configuration Manager host 28 PASS
Ruff                        PASS
git diff --check            PASS
Manager 0.3.18 rg           EMPTY
```

The final Git HEAD for this local delta was not supplied in this chat. Do not invent one.

## Alarm Configuration workspace CURRENT

Alarm-specific responsibility remains:

```text
Save Draft
→ Confirmed Tool Catalog required
→ current catalog revision pinned into workspace payload
```

Generic workspace responsibility is delegated to:

```text
ManagerWorkspaceBinding
```

This removes duplicated owner/source/snapshot/document mechanics while preserving the Command Center domain rule.

No legacy wrapper or compatibility alias remains.

## Identidad y persistencia de Alarm Configuration — CURRENT

```text
ALARM_CONFIGURATION_SOURCE_KEY = 'alarm-configuration'
```

Physical projection identity:

```text
logical_id       ada.command_center.alarms.configuration.projection
physical_name    alarm-configuration
Cosmos PK        /partition_key
```

Local projection:

```text
<base_root>/conciencia_situacional/command-center/projections/alarm-configuration/
```

Durable Cosmos container:

```text
alarm-configuration
```

Tool Catalog continues to use Storage even under the local Manager provider.

## C2 preservado — procesos y volumen

`APPLICATION=ada-command-center` identifica Materialization, Runtime y Delivery, que conservan `job_key` y leases propios.

`VOLUMEN_PATH` continúa manual, absoluta y debe referirse al mismo montaje físico.

La raíz operacional continúa:

```text
VOLUMEN_PATH/ada-command-center/alarms
```

Endpoint/base/credential Cosmos must still match the durable host physically. That E2E equivalence remains UNVERIFIED.

## Pipeline CURRENT preservado

1. Source v3 congela `AlarmConfigurationSnapshot(configuration, tool_dependencies)` con referencias Rn/Cn.
2. Materialization obtiene ProjectionRecord y qualification manual externa; publica READY íntegro o diagnóstico BLOCKED.
3. Runtime adopta pin exacto mediante WAL/EFFECTIVE.
4. Runtime publica CURRENT v1 y FACTS v2 por canales separados.
5. Delivery consume sólo el último CURRENT y exige alineación con EFFECTIVE/READY.

Manager convergence did not change these contracts.

## Web application boundary

CURRENT:

```text
ada-command-center-configuration-manager
→ temporary standalone Manager host
```

NOT CURRENT:

```text
ada-command-center-generic-application
→ not implemented
```

Therefore dual-product distribution cannot be closed yet.

## OPEN separados

- **Command Center Generic Application composition:** PLANNED / NEXT.
- **Dual-product tooling/distribution:** PLANNED / BLOCKED until Generic Application exists.
- **Resource Preparation + startup gate:** PLANNED / DEFERRED.
- **Tool Catalog local filesystem:** NOT IMPLEMENTED.
- **C3/C5 qualification/evidence:** OPEN according to their owners.
- **Docker/Azure final runtime:** UNVERIFIED.
- **Materialization ↔ Web Projection Cosmos physical E2E:** UNVERIFIED.
- **Live:** NOT IMPLEMENTED.
- **Management Capture/Projection, History/Analytics:** PLANNED / SEPARATE.
- **UX and END_OF_SHIFT operational work:** OPEN / SEPARATE.
- **Python migration:** BLOCKED / DEFERRED.

No new functionality is authorized by this documentation update.
