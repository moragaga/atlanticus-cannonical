# Alarm Engine — Runtime Adoption and Effective Configuration

Estado: **PROJECT CONTRACT AGREED / NOT YET IMPLEMENTED**

## Authority checkpoint

Implementación auditada:

```text
moragaga/atlanticus:main
ebf736a1cf5193a297fbafc55c5c11ca9993f24c
```

Canonical base:

```text
moragaga/atlanticus-cannonical:main
ed49507dbfd808585eb0eb9b89ad1f48b8b3f5a5
```

Decisions consultado:

```text
moragaga/atlanticus-decisions:main
50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

Este documento consolida un contrato de Project todavía no implementado. No reemplaza las decisiones registradas; concreta la frontera Runtime necesaria para hacer operativa la separación `READY != EFFECTIVE` ya registrada.

## 1. Propósito

B.2 Materialization produce candidatos READY.

Runtime Adoption determina si y cómo un candidato READY pasa a EFFECTIVE sin reinterpretar o perder hot state durable.

La configuración EFFECTIVE debe existir como estado global explícito, no inferirse desde el último commit de un priority group.

## 2. AlarmResolutionKey

Contrato compartido:

```text
AlarmResolutionKey
    alarm_configuration_revision
    confirmed_tool_catalog_revision
```

Es la identidad exacta de una resolución B.2.

No agregar `evaluator_registry_revision`, deployment id ni runtime build a este key sin una decisión específica.

## 3. Effective Configuration Head

PROJECT CONTRACT AGREED:

```text
AlarmEffectiveConfigurationHead
    resolution_key: AlarmResolutionKey
    effective_at: datetime
    adoption_id: str
```

Responde:

> ¿Qué resolución adoptó Runtime exitosamente y debe usar el resto del sistema como configuración operacional vigente?

No contiene el artifact completo.

## 4. READY != EFFECTIVE

```text
B.2 READY
-> candidate coherent

Runtime Adoption succeeds
-> EFFECTIVE
```

Un candidato puede ser READY y posteriormente ser rechazado por limitaciones de transición Runtime. En ese caso:

```text
Effective Head unchanged
Delivery remains on previous exact key
Management Capture remains on previous exact key
```

## 5. Effective es global

El Effective Head es único para la configuración completa.

No existe:
- effective por Rule;
- effective por priority group;
- Runtime effective distinto de Delivery effective.

Runtime y Delivery artifacts nacen de la misma resolution y comparten el mismo key.

## 6. Group snapshot provenance != Effective Head

CURRENT persistence materializa hot snapshots por `priority_group`.

`GroupRuntimeSnapshot.state_basis` describe bajo qué configuración ocurrió la última mutación durable relevante de ese hot state.

El Effective Head describe qué configuración está vigente globalmente ahora.

Por tanto pueden diferir legítimamente.

Ejemplo:

```text
R20/T42
hot state mutates
snapshot basis = R20/T42

R21/T42
only Delivery visibility changes
Adoption succeeds
Effective Head = R21/T42
snapshot basis remains R20/T42
```

No generar commits vacíos para sincronizar metadata.

## 7. Occurrence provenance

Una occurrence conserva la provenance bajo la cual nació:

```text
resolution_key_at_start
```

Adoptions posteriores no la reescriben.

Esto permite distinguir historia de creación y configuración global actual.

## 8. Configuration Adoption Commit

PROJECT CONTRACT AGREED:

```text
ConfigurationAdoptionCommit
    adoption_id
    previous_resolution_key: AlarmResolutionKey | None
    target_resolution_key: AlarmResolutionKey
    effective_at
    affected_group_commit_ids
```

La shape física puede refinarse durante implementación, pero estas responsabilidades quedan congeladas.

`previous_resolution_key` actúa como compare-and-set semántico: la Adoption no puede publicarse sobre un head distinto al que planificó.

## 9. Same Runtime WAL

No crear un segundo journal de Adoption.

La persistencia Runtime debe evolucionar para poder representar records discriminados dentro del mismo WAL durable/recoverable:

```text
Alarm Runtime Journal
├── GroupStateCommit
├── GroupStateCommit
└── ConfigurationAdoptionCommit
```

No usar un priority group sintético como `__configuration__`.

## 10. Orden del batch de Adoption

Conceptualmente:

```text
1. group reconciliation commits
2. ConfigurationAdoptionCommit
```

El record de Adoption es el último record lógico y declara que el target pasa a EFFECTIVE.

Una Adoption que no requiere hot-state mutations puede tener:

```text
0 GroupStateCommit
1 ConfigurationAdoptionCommit
```

Esto es necesario para cambios sólo de Delivery/metadata.

## 11. Crash safety y recovery

Se reutiliza el protocolo CURRENT:

```text
append WAL
-> publish durable head
-> materialize read models/snapshots
-> publish materialized head
```

### Crash antes de durable publication

Tail no confirmado se descarta. Effective Head anterior permanece.

### Crash después de durable publication pero antes de materialization completa

```text
journal.durable != journal.materialized
```

Runtime queda en `RECOVERY REQUIRED`.

Recovery materializa records pendientes, incluido el `ConfigurationAdoptionCommit`, y recién entonces el Effective Head avanza.

Un snapshot parcialmente reconciliado mientras el journal está desalineado no es un estado operacional consumible.

## 12. Precondición operacional

Congelado:

```text
journal.durable == journal.materialized
```

es precondición para:
- nueva Runtime execution;
- nueva Runtime Adoption;
- nueva Management Capture operacional;
- nueva Live Projection materialization.

La última Live Projection ya publicada puede permanecer disponible durante recovery, pero no se publica una nueva a partir de estado parcial.

## 13. Effective Head como materialized read model

El Effective Head se materializa desde el WAL confirmado; no se escribe como verdad independiente fuera del protocolo.

Conceptualmente:

```text
ConfigurationAdoptionCommit
        |
        v
runtime/state/effective-configuration.json
```

La verdad durable sigue siendo el WAL confirmado.

## 14. Exact-key consumption

Delivery nunca usa:
- latest READY;
- highest revision;
- newest Delivery artifact.

Usa exactamente:

```text
EffectiveHead.resolution_key
        |
        v
DeliveryConfiguration[exact key]
```

Management Capture usa la misma regla.

Si falta el artifact exacto:
- no hacer fallback a otra revision;
- Delivery no produce una nueva proyección con semántica mezclada;
- Management Capture no acepta nuevas gestiones dependientes de esa configuración.

Puede mantenerse la última Live Projection ya publicada.

## 15. Runtime startup

Target startup:

```text
1. recover Runtime WAL
2. require journal aligned
3. read Effective Configuration Head
4. load RuntimeAlarmConfiguration[exact resolution_key]
5. resolve deployed evaluators
6. build AlarmExecutionSession
7. hydrate group snapshots
8. begin iterations
```

Runtime no descubre “latest configuration” en SharePoint ni consume “latest READY”.

## 16. Bootstrap

Antes de la primera Adoption:

```text
Effective Head = None
```

Primera Adoption:

```text
previous_resolution_key = None
target_resolution_key = R1/T1
```

Si existe durable hot history pero no Effective Head, no inferir silenciosamente la configuración. Clasificar como migration required/corruption según la estrategia de implementación.

## 17. Adoption universe

CURRENT Adoption clasifica únicamente `source.session.identities`, lo que no cubre correctamente Rules nuevas o disabled->enabled.

TARGET:

```text
source.defined_alarm_identities
UNION
target.defined_alarm_identities
```

La transición se clasifica sobre presencia definida + presencia ejecutable.

## 18. Adoption dispositions target

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

### ADDED

```text
source: not defined
target: defined
```

Si además es executable, entra a evaluación bajo la nueva configuración. No crea occurrence durante Adoption; la occurrence nace si una evaluación posterior resulta ACTIVE.

Una Rule nueva pero disabled puede no requerir hot-state mutation.

### ENABLED

```text
source: defined, not executable
target: defined, executable
```

Empieza a evaluar desde la nueva revisión. No crea occurrence automáticamente.

### DISABLED

```text
source: executable
target: defined, not executable
```

Occurrence abierta se cierra con `CONFIGURATION_DISABLED`.

### REMOVED

```text
target: not defined
```

Occurrence abierta se cierra con `CONFIGURATION_REMOVED`.

### COMPATIBLE

Conserva occurrence/episode/hot state y aplica nueva semántica compatible desde Adoption/siguiente ciclo.

### STRUCTURAL_RESET

Cambio válido que requiere recomposición estructural del grupo.

### REJECTED

La configuración puede ser B.2 READY pero Runtime CURRENT no soporta la transición. EFFECTIVE no avanza.

## 19. Reappearance reconciliation

Cambios target a:
- `reappearance_after_seconds`;
- `reappearance_special_conditions`;

deben impedir clasificación `UNCHANGED`.

Timer changes son `COMPATIBLE + reconcile current effect`.

Si existe ManagementEffect:

```text
new due = management_effect.effective_at + target timer
```

Si el nuevo due ya venció:

```text
reappearance at adoption effective_at
```

No retroactivamente.

Cambio a timer `None` mantiene el ManagementEffect sin deadline temporal. Cambio desde `None` puede disparar inmediatamente si el nuevo due teórico ya venció.

Special Condition refs nuevas aplican level-triggered desde la configuración adoptada.

## 20. Visibility-only / Delivery-only Adoption

Un cambio sólo de:
- visibility;
- display metadata;
- Messages;
- deactivation policy futura;
- visual targets;
- process projection mode;

puede requerir cero hot-state mutations.

Aun así Adoption debe registrar:

```text
previous key -> target key
```

y avanzar Effective Head.

EFFECTIVE es estado de configuración global, no una propiedad derivada de que haya ocurrido un Engine state mutation.

## 21. CURRENT gaps que este contrato no oculta

CURRENT todavía:
- no tiene Effective Configuration Head;
- no tiene journal record discriminado;
- no persiste Adoption global con cero group commits;
- usa `alarm_configuration_revision + tool_registry_revision` separados;
- clasifica cambios sólo sobre source executable identities;
- no implementa `ADDED`/`ENABLED`;
- no reconcilia timer/SC refs target como contrato de Adoption.

## 22. OPEN de implementación

- schema físico/version exacta del journal discriminado;
- generation de `adoption_id`;
- location/schema exacto del Effective Head;
- migration desde persistence CURRENT;
- tests de crash/recovery para Adoption global;
- integración con artifact stores Runtime/Delivery.

No resolver estos detalles con aliases legacy ni adapters temporales.
