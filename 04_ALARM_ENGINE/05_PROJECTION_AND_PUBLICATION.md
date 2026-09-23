# Alarm Engine — Projection and Publication

Estado: **CURRENT / BASE SNAPSHOT V3 IMPLEMENTED / OPERATIONAL COSMOS PROJECTION NEXT**

## Capas distintas

No mezclar:

```text
Alarm Source/Release
Alarm Configuration base Projection
B.2 Runtime Configuration
B.2 Delivery Configuration
Alarm Live Projection
Alarm Management Projection
```

## Alarm Source CURRENT

```text
AlarmConfigurationSnapshot
    configuration: AlarmConfiguration
    tool_dependencies: ToolDependencyManifest
```

Source schema:

```text
ada_command_center_alarm_configuration_release
schema_version = 3
```

No existe decoder legacy v2.

## Base Projection CURRENT

La Projection base conserva exactamente el snapshot publicado y no vuelve a consultar Tools.

```text
Alarm Source release Rn
-> AlarmConfigurationSnapshot(Rn, Cn)
-> base Projection payload = same snapshot
```

La Projection base no produce Runtime/Delivery artifacts y no decide `READY` ni `EFFECTIVE`.

## Operational Cosmos Projection — NEXT

Todavía no existe una proyección operacional específica de Alarm Configuration a Cosmos en
`scopes/ada-command-center`.

Siguiente incremento:

```text
AlarmConfigurationSnapshot v3
-> durable operational Projection in Cosmos
```

Debe preservar:
- source release;
- ToolDependencyManifest;
- confirmed Tool revision;
- payload completo del snapshot.

No debe volver a consultar Tool Catalog al proyectar.

## B.2 Runtime / Delivery

Una resolución coherente produce Runtime + Delivery con la misma `AlarmResolutionKey`.

```text
READY != EFFECTIVE
```

Runtime Adoption controla `EFFECTIVE`.

## Live vs Management

Live representa current operational state.
Management Projection representa historical user management activity.

No fusionarlas.

## Storage boundary

Para dominios migrados, Storage/Blob es autoridad durable.

```text
Confirmed Tool Catalog -> Storage -> END
```

No proyectar el consolidado Tool de vuelta a Cosmos.

Cosmos puede ser superficie operacional de consumo para Alarm Configuration sin cambiar Source
authority.
