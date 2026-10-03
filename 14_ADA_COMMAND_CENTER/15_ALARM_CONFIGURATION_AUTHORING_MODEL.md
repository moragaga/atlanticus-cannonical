# ADA Command Center — Alarm Configuration Authoring Model

Estado: **CURRENT — authoring contract migrated to `ada-contracts`; Web qualification GREEN in current cutover; broader Configuration Manager gate BLOCKED elsewhere**.

## Shared contract ownership CURRENT

`ada-contracts-alarms` owns:

```text
AlarmIdentity
Alarm definition/configuration models
AlarmConfiguration
AlarmConfigurationSnapshot
shared validation/errors
```

`ada-contracts-tools` owns:

```text
Tool enums/structure/source contracts
ToolDependencyManifest
```

`ada-command-center/domain/alarms` conserva sólo:

```text
ALARM_CONFIGURATION_SOURCE_KEY
routing policy
```

## Published aggregate CURRENT

```text
AlarmConfigurationSnapshot
    configuration: AlarmConfiguration
    tool_dependencies: ToolDependencyManifest
```

Family sigue derivándose de `AlarmIdentity.family_key`; no crear aggregate durable paralelo sólo por UI.

## Authoring ownership DECIDED

Command Center es owner de:

```text
authoring
semantic/business validation
Tool catalog reference resolution
routing validation
visual target validation
publication
```

Un snapshot publicado debe ser válido/materializable. No delegar semantic qualification downstream para compensar un authoring incompleto.

## Save / Validate / Publish

Conservar exact Tool revision Cn durante el workflow y congelar `ToolDependencyManifest(Cn)` al publicar.

```text
VALID_AT_SAVE != EFFECTIVE
```

pero publicación válida sí implica que Materialization no debe volver a descubrir o reinterpretar Tools.

## Routing CURRENT

```text
PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC -> END
```

Policy ubicada en Command Center domain y basada en `ada.contracts.tools.ToolConfigurationKind`.

## Deactivation CURRENT

Se preserva el contrato vigente:

```text
1..11 horas
END_OF_SHIFT
```

No reinterpretar `END_OF_SHIFT` como timestamp operacional sin provider de turno/calendario.

## Qualification observada

`web/alarms/configuration` alcanzó:

```text
123 tests PASS
Ruff GREEN
build GREEN
```

durante el cutover actual.

Esto no acredita acceptance browser completa ni durable smoke.

## OPEN

- defectos UX históricos que afecten integridad real del draft;
- recovery/acceptance durable;
- cleanup downstream de Materialization para eliminar semantic resolution duplicada;
- contrato operacional de `END_OF_SHIFT`;
- full Command Center Configuration Manager parity, bloqueante para terminar el gate actual.
