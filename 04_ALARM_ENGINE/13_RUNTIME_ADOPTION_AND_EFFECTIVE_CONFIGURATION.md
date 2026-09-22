# Alarm Engine — Runtime Adoption and Effective Configuration

Estado: **PROJECT CONTRACT AGREED / ALARM RESOLUTION KEY + REAPPEARANCE RUNTIME SHAPE IMPLEMENTED / ADOPTION NOT YET IMPLEMENTED**

## Authority checkpoint

Implementación auditada:

```text
moragaga/atlanticus:main
cd08bd8d2c25bd89eb39fa15cbda209c8e9be617
```

Canonical base:

```text
moragaga/atlanticus-cannonical:main
56943d94889719544f426322ded4a877245dfaee
```

Decisions consultado:

```text
moragaga/atlanticus-decisions:main
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

## 1. Propósito

B.2 Materialization produce candidatos READY.

Runtime Adoption determina si y cómo un candidato READY pasa a EFFECTIVE sin perder hot state durable.

```text
READY != EFFECTIVE
```

## 2. AlarmResolutionKey CURRENT

Ya implementado en Alarm Core:

```text
AlarmResolutionKey
    alarm_configuration_revision
    confirmed_tool_catalog_revision
```

Materialization Runtime/Delivery artifacts usan este mismo VO.

No agregar evaluator revision, deployment id ni runtime build.

## 3. Effective Configuration Head — PLANNED

```text
AlarmEffectiveConfigurationHead
    resolution_key: AlarmResolutionKey
    effective_at: datetime
    adoption_id: str
```

No implementado.

Es global: no existe effective por Rule ni por priority group.

## 4. Exact-key consumption

Contrato congelado:

```text
EffectiveHead.resolution_key
        |
        +--> RuntimeAlarmConfiguration[exact key]
        +--> DeliveryAlarmConfiguration[exact key]
        +--> Management Capture[exact key]
```

No fallback a latest/highest/newest.

## 5. Occurrence provenance — OPEN

Target:

```text
resolution_key_at_start
```

CURRENT todavía conserva revisions históricas separadas.

No introducir alias permanente durante la migración.

## 6. Configuration Adoption Commit — PLANNED

```text
ConfigurationAdoptionCommit
    adoption_id
    previous_resolution_key: AlarmResolutionKey | None
    target_resolution_key: AlarmResolutionKey
    effective_at
    affected_group_commit_ids
```

Debe vivir en el mismo WAL Runtime, no en journal paralelo ni priority group sintético.

## 7. Crash safety

Contrato permanece:

```text
append WAL
-> publish durable head
-> materialize read models/snapshots
-> publish materialized head
```

Precondición operacional:

```text
journal.durable == journal.materialized
```

para nueva Runtime execution, Adoption, Management Capture operacional y nueva Live materialization.

## 8. Adoption universe

Target:

```text
source.defined_alarm_identities
UNION
target.defined_alarm_identities
```

Dispositions target:

```text
UNCHANGED
COMPATIBLE
ADDED
ENABLED
DISABLED
REMOVED
STRUCTURAL_RESET
REJECTED
```

No implementadas completamente.

## 9. Reappearance Runtime shape CURRENT

Core ya expone:

```text
PlannedAlarm.reappearance_after_seconds: int | None
PlannedAlarm.reappearance_special_conditions: tuple[AlarmIdentity, ...]
```

El pure B.2 resolver debe convertir authored minutos a Runtime segundos.

Esto elimina el gap de shape Runtime, pero no implementa la reconciliación durante Adoption.

## 10. Reappearance reconciliation — OPEN

Cuando una revisión EFFECTIVE cambie:

```text
reappearance_after_seconds
or
reappearance_special_conditions
```

Adoption debe decidir cómo reconciliar un ManagementEffect/hot state existente.

La base histórica exige recalcular timer y aplicar referencias nuevas a la occurrence gestionada
vigente; el contrato detallado de transición y su crash/recovery qualification siguen pendientes.

No implementar esta reconciliación dentro del pure B.2 resolver.

## 11. Delivery-only changes

Un cambio sólo de visibility/display/Messages/deactivation policy/visual targets puede requerir
cero hot-state mutation y aun así debe avanzar Effective Head mediante Adoption durable.

## 12. CURRENT gaps

- no Effective Configuration Head;
- no ConfigurationAdoptionCommit;
- no journal discriminado;
- no global Adoption durable con cero group commits;
- occurrence provenance no usa `resolution_key_at_start`;
- `PlannedAlarm` todavía mantiene revisions históricas;
- ADDED/ENABLED incompletos;
- reappearance hot-state reconciliation no implementada;
- integration con Materialization artifact stores inexistente.

## 13. Estado respecto de B.2 contracts

Ya CURRENT:
- `AlarmResolutionKey`;
- `RuntimeAlarmConfiguration`;
- `DeliveryAlarmConfiguration`;
- `AlarmConfigurationResolution`;
- `PlannedAlarm.reappearance_after_seconds`.

Esto no vuelve EFFECTIVE ningún candidate.

## 14. OPEN de implementación

- schema/version del journal discriminado;
- generation de `adoption_id`;
- Effective Head store/materialization;
- migration desde persistence CURRENT;
- crash/recovery qualification;
- Runtime/Delivery artifact stores;
- adoption transition gaps;
- reappearance reconciliation.

No resolver con adapters legacy.

## 15. Foco actual

Runtime Adoption no es el siguiente incremento.

Siguiente:

```text
PURE B.2 ALARM CONFIGURATION RESOLVER
```
