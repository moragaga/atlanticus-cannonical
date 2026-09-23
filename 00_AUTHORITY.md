# Atlanticus — Authority

Estado: **CURRENT**

## Autoridades activas

### Implementación

```text
moragaga/atlanticus:main
880cb692054c2cd78cdc29cf62ab7b16bbd2c3d6
```

Ese SHA es la realidad implementada CURRENT observada al cierre.

El último commit semántico de este hito Alarm/Tool es:

```text
d2a5e14822d3711e64668b8e70cfa15d7ddae2f0
```

Los dos commits posteriores hasta `880cb692054c2cd78cdc29cf62ab7b16bbd2c3d6` modifican `operational-data` y tooling,
sin cambiar los contratos de ADA Command Center Alarm Configuration cerrados aquí.

### Canonical

```text
moragaga/atlanticus-cannonical:main
```

Checkpoint inspeccionado antes de este reemplazo:

```text
148b178df74ee3083681140f3bb7997a02435b80
```

### Historical decisions

```text
moragaga/atlanticus-decisions:main
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

`atlanticus-decisions` preserva genealogía e intención histórica.
No prevalece sobre implementación CURRENT ni sobre canonical actualizado.

## Jerarquía

1. `atlanticus:main` — realidad implementada.
2. `atlanticus-cannonical:main` — contratos, fronteras y estado vigente.
3. qualification/tests vigentes — evidencia.
4. decisiones explícitas del Project todavía no canonizadas — delta temporal.
5. `atlanticus-decisions` — historia.
6. memoria/conversaciones — pista.

## Git

Git permanece **SOLO LECTURA** por defecto.

No crear commits, push, ramas, PR, issues ni otra mutación remota sin autorización explícita.

## Cierre CURRENT — Alarm Tool Dependency Manifest v3

```text
COMMAND-CENTER-TOOLS-DOMAIN                    CLOSED / VERIFIED / CURRENT
ALARM-TOOL-DEPENDENCY-MANIFEST                 CLOSED / VERIFIED / CURRENT
ALARM-CONFIGURATION-SNAPSHOT-V3                CLOSED / VERIFIED / CURRENT
ALARM-TOOLS-VALIDATE-PUBLISH-FREEZE            CLOSED / VERIFIED / CURRENT
PURE-B.2-RESOLVER                              CLOSED / VERIFIED / CURRENT

ALARM-CONFIGURATION-COSMOS-PROJECTION          PLANNED / NEXT
B.2-MATERIALIZATION-PROCESS                    PLANNED / AFTER
RUNTIME-ADOPTION-EFFECTIVE-HEAD                PLANNED
ALARM-LIVE-DELIVERY                            PLANNED
```

## Tool authority CURRENT

```text
multiple upstream Tool projections/Cosmos
        +
prior/current Storage state
        |
        v
Tool reconciliation / controlled certification
        |
        v
Confirmed Tool Catalog Cn
        |
        v
Storage
        |
       END
```

No existe como target:

```text
Confirmed Tool Catalog -> Command Center Cosmos
```

La ausencia de esa proyección Cosmos es intencional.

## Alarm Configuration persistence CURRENT

Authored aggregate:

```text
AlarmConfiguration
    rules
    messages
```

Snapshot durable:

```text
AlarmConfigurationSnapshot
    configuration
    tool_dependencies: ToolDependencyManifest
```

La revisión Tool no se duplica:

```text
AlarmConfigurationSnapshot.confirmed_tool_catalog_revision
==
AlarmConfigurationSnapshot.tool_dependencies.revision
```

Source contract:

```text
document_type = ada_command_center_alarm_configuration_release
schema_version = 3
```

Schema v2 queda **SUPERSEDED**.
No existe decoder legacy v2.

## Tool Dependency Manifest CURRENT

Owner:

```text
scopes/ada-command-center/domain/tools
ada-command-center-tools-domain==1.0.0
```

Entry:

```text
ToolDependencyEntry
    tool_key
    display_name
    source_release_id
    kind
    structure: ToolStructure
```

Manifest:

```text
ToolDependencyManifest
    confirmed_tool_catalog_revision
    tools
```

El manifest conserva nombres y estructura histórica para no depender de contratos Tool futuros.

## Freeze validate -> publish CURRENT

El Manager genérico permanece intacto.

Alarm Configuration usa un sidecar específico del workspace:

```text
_confirmed_tool_catalog_revision = Cn
```

Regla:

```text
Save Draft
-> fija Cn current

Validate
-> exige Cn todavía current
-> exige que todas las Tool references definidas existan

Publish
-> vuelve a exigir Cn todavía current
-> captura manifest desde esa misma revisión
-> persiste AlarmConfigurationSnapshot v3
```

Si Tools cambia `Cn -> Cn+1` entre validation y publication:

```text
publication -> BLOCKED
```

No se enlaza silenciosamente `Cn+1`.

## B.2 exact evidence CURRENT

B.2 no debe resolver una Alarm revision guardada contra `latest Tool Catalog`.

Para una Alarm source release `Rn`:

```text
Rn
-> AlarmConfigurationSnapshot
-> ToolDependencyManifest(Cn)
```

Materialization usa esa evidencia exacta.

La aparición de `C2` no invalida `R1/C1`.
Una nueva publicación Alarm adopta la revisión Tool current sólo mediante el flujo explícito de
workspace/validation/publication.

## Decisions históricas refinadas

Quedan **SUPERSEDED / REFINED** las formulaciones históricas que implicaban:

```text
same Alarm revision R1
+ later Tool Catalog C2
-> re-resolve R1/C2 without a new Alarm publication
```

También queda superada la topología histórica:

```text
Confirmed Tool Catalog -> Command Center Cosmos
```

y SharePoint como autoridad física general en dominios ya migrados.

`LATEST SAVED = LATEST VALID_AT_SAVE` permanece, pero:

```text
VALID_AT_SAVE != B.2 READY != EFFECTIVE
```

El save/publish gate CURRENT valida aggregate + Tool correlation/existence.
Evaluator qualification y demás qualification B.2 siguen perteneciendo a Materialization.

## Qualification observada del hito

```text
domain/tools
8 passed
ruff check GREEN
ruff format GREEN

domain/alarms
50 passed
ruff check GREEN
ruff format GREEN

web/alarms/configuration
35 passed
ruff check GREEN
ruff format GREEN

configuration-manager
11 passed
ruff check GREEN
ruff format GREEN
```

## Conflictos abiertos

```text
Project baseline Python           3.14.7
Command Center package metadata  3.14.2
```

OPEN / SEPARATE.

También queda diferida la normalización de ownership entre `domain/tools` y `ada-web-tools`.

## Siguiente foco único

```text
ALARM CONFIGURATION OPERATIONAL PROJECTION TO COSMOS
```

Primero diseñar contrato/store/composición de la proyección del snapshot v3 autocontenido.
No mezclar todavía con Materialization Process, Runtime Adoption, Live Delivery o Management Capture.
