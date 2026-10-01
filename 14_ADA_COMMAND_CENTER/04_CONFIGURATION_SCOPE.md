# ADA Command Center — Configuration Scope

Estado: **CURRENT — Alarm Configuration Snapshot Source v3 / Tool dependencies Rn/Cn / MANAGER WORKSPACE CORE CONVERGED LOCALLY; Durable E2E and Resource Preparation remain UNVERIFIED/PLANNED.**

## Authority of this close

```text
Last confirmed Atlanticus HEAD:
moragaga/atlanticus@36361dd570f86e8350ea4a6ee0e09bab351ba171

Alarm Configuration Manager convergence delta:
VERIFIED LOCAL / PENDING FINAL GIT HEAD
```

Command Center administra Alarm Configuration reutilizando `atlanticus.web.manager` sin modificar semántica de Manager genérico.

## Aggregate y publicación CURRENT

```text
AlarmConfiguration
    rules
    messages

AlarmConfigurationSnapshot
    configuration: AlarmConfiguration
    tool_dependencies: ToolDependencyManifest
    schema_version: 3
```

Definiciones Tool no se embeben en configuración Alarm editable.

El workspace conserva sidecar `_confirmed_tool_catalog_revision`, no miembro de `AlarmConfiguration`.

Source schema v2 está SUPERSEDED sin decoder legacy.

### Flujo vigente

```text
Save Draft -> consulta Confirmed Tool Catalog y fija Cn en workspace
Validate   -> revisa configuración/Cn actual y referencias Tool
Verify     -> concurrencia de Source bajo Manager
Publish    -> vuelve a comprobar Cn (drift guard) y congela manifest Cn
```

El manifest incluye origins, escalones de routing definidos y visual targets según contrato actual. Cambios Cn posteriores no reinterpretan snapshots Rn/Cn inmutables.

```text
VALID_AT_SAVE != READY != EFFECTIVE
```

## Manager workspace convergence — CURRENT locally

Package:

```text
ada-command-center-web-alarm-configuration==0.1.1
atlanticus-web-manager==0.3.19
```

`AlarmConfigurationManagerWorkspaceBinding` conserva sólo la regla de dominio que le pertenece:

```text
Confirmed Tool Catalog required
→ pin current catalog revision into payload
```

Después delega al core:

```text
ManagerWorkspaceBinding
→ owner validation
→ SourceKey validation
→ SourceSnapshot base
→ workspace document parsing/serialization
→ payload copy/update
```

No existe shim para mensajes, schemas o comportamiento del binding anterior.

La excepción genérica de workspace inválido pertenece al Manager core.

Qualification local del paquete:

```text
124 PASS
Ruff PASS
```

## C1 — Tool ownership CURRENT

`web/tools/catalog` construye/persiste Confirmed Tool Catalog CURRENT en Blob; `web/tools/discovery-cosmos` inspecciona conexiones Tool Cosmos nombradas y confirma revisiones; `web/tools/catalog-manager` posee UI/callbacks. Host temporal compone services; `backend/tools` está SUPERSEDED. El editor no reinterpreta snapshots congelados con Tool latest.

Tool Catalog Manager local convergence:

```text
ada-command-center-web-tool-catalog-manager==0.1.1
atlanticus-web-manager==0.3.19
9 PASS
Ruff PASS
```

**Límite actual:** `ADA_MANAGER_PERSISTENCE_PROVIDER=local` no convierte todavía Tool Catalog a filesystem. El host temporal sigue requiriendo Storage para Tool Catalog en ambos providers.

## C2 — Source Key y topología CURRENT

`domain/alarms/identity.py` define:

```text
ALARM_CONFIGURATION_SOURCE_KEY = 'alarm-configuration'
```

Web lo transforma en `SourceKey` técnico y Materialization/Runtime/Delivery lo consumen. La constante no elimina verificaciones del `source_key` persistido.

Identidad física compartida:

```text
web/alarms/configuration/resources.py
ALARM_CONFIGURATION_PROJECTION_PHYSICAL_NAME = 'alarm-configuration'
```

Resource contract Cosmos:

```text
logical_id        ada.command_center.alarms.configuration.projection
physical_name     alarm-configuration
partition_key     /partition_key
allowed_override  CONNECTION_REF
```

El nombre anterior:

```text
ada-command-center-alarm-configuration-projection
```

está **SUPERSEDED**. No mantener alias ni compatibilidad legacy.

## Namespace Storage/local CURRENT

```text
application_namespace = conciencia_situacional
tool_namespace        = command-center
```

Storage/Source:

```text
conciencia_situacional/command-center/tool-catalog/current.json
conciencia_situacional/command-center/sources/alarm-configuration/...
```

Projection local:

```text
<base_root>/conciencia_situacional/command-center/projections/alarm-configuration/...
```

Cosmos:

```text
alarm-configuration
PK /partition_key
```

No generalizar esta identidad a recursos no implementados.

## Temporary host CURRENT

```text
ada-command-center-configuration-manager==0.1.1
atlanticus-web-manager==0.3.19
```

El host sigue siendo temporal y standalone para desarrollo/qualification.

Qualification local:

```text
28 PASS
Ruff PASS
```

No equivale al futuro Command Center Generic Application.

## Configuración física todavía OPEN

Cuenta/base/credencial Cosmos de entrada Materialization siguen siendo configuradas externamente y deben coincidir físicamente con el host durable: UNVERIFIED.

El contenedor Blob permanece ambiental.

Las conexiones Tool Cosmos siguen siendo múltiples/nombradas cuando corresponde.

`APPLICATION=ada-command-center` continúa común entre los tres jobs.

`VOLUMEN_PATH` absoluta/compartida la define el operador y no se deriva de Source.

## Evidencia preservada de hitos anteriores

El cierre de naming físico anterior reportó:

```text
Alarm Configuration Web                         123 PASS
Alarm Projection Cosmos                           5 PASS
ADA Command Center Configuration Manager         28 PASS
TOTAL                                            156 PASS
git diff --check                                 PASS
```

La convergencia Manager posterior reportó:

```text
Alarm Configuration Web                         124 PASS
Tool Catalog Manager                              9 PASS
ADA Command Center Configuration Manager         28 PASS
Ruff                                             PASS
git diff --check                                 PASS
atlanticus-web-manager==0.3.18 under CC Web      NONE
```

Estas evidencias no certifican Source Blob + Cosmos E2E, Docker, Azure ni equivalencia física multi-host.

## OPEN y frentes distintos

- **Command Center Generic Application:** PLANNED / NEXT; no implementada.
- **Resource Preparation + startup gate:** PLANNED / DEFERRED hasta cerrar application composition.
- **Tool Catalog local:** NOT IMPLEMENTED; no declarar `local` totalmente filesystem.
- **C3:** productor/verificadores GREEN reales; `ALARM_QUALIFICATIONS_FILE` manual continúa CURRENT.
- **C5:** contract key/version de technical evidence y auditoría `.env.detail`.
- **Docker/distribución:** UNVERIFIED.
- **UX/END_OF_SHIFT operacional:** frente separado.

No utilizar este cierre documental para crear containers, variables, servicios o adapters no existentes.
