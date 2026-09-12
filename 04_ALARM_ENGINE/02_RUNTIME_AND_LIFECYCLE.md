# Alarm Engine — Runtime and Lifecycle

Estado: **IMPLEMENTED**

Implementación auditada:
`scopes/ada-command-center/backend/processes/alarms-runtime`

## Frontera

El runtime ejecutable está fuera de `alarms/core`.

Módulos actuales incluyen:

- adoption
- adoption_execution
- commit
- composition
- cycle
- durability
- inputs
- iteration
- job_composition
- session
- snapshot

## Ciclo operacional

`AlarmOperationalCycle.execute`:

1. valida contexto/sesión/inputs;
2. carga estado durable;
3. reúne grupos relevantes;
4. evalúa Rules;
5. resuelve priority por grupo;
6. aplica management cascade/lifecycle;
7. materializa commits;
8. persiste batch durable.

Incluye:
- grupos configurados;
- grupos con deactivation pendiente;
- snapshots existentes aunque cambie config.

## Commit

Los commits cargan trazabilidad como:
- configuration revision;
- tool registry revision;
- runtime artifact revision;
- technical evidence;
- previous commit.

No se acepta publicar dos veces la misma iteración durable.

## Regla

No trasladar al Web:
- cálculo de prioridad;
- interpretación de AlarmDefinition;
- resolución de deactivation/management;
- lógica de lifecycle.

El Web consume resultados ya resueltos.
