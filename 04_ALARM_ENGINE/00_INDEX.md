# Alarm Engine — Index

Estado: **CURRENT — shared contracts cutover IMPLEMENTED; qualification del cutover IN PROGRESS; extracción física del Engine PLANNED**.

Checkpoint de implementación:

```text
atlanticus@6725237a19c4442fdfa1b32c3410c124e9348dbc
```

## Ownership CURRENT

```text
ada-contracts-tools
    Tool shared contracts

ada-contracts-alarms
    Alarm shared contracts + Engine publication schemas

ada-command-center/domain/alarms
    ALARM_CONFIGURATION_SOURCE_KEY
    routing policy

ada-command-center/backend/alarms + processes
    Engine/materialization/persistence/runtime/delivery implementation
```

`scopes/ada-command-center/domain/tools` está **SUPERSEDED / REMOVED**.

## Estado del cutover

La migración de consumidores de Command Center hacia `ada-contracts` está implementada en `main` y parcialmente calificada.

El gate avanzó hasta `ada-command-center-configuration-manager`, donde quedó **BLOCKED** por drift previo de Command Center frente a las capacidades genéricas actuales de Users/Profiles/Navigation/Manager. El bloqueo no demuestra un fallo de los contratos de Alarmas.

## Documentos

| Archivo | Rol actual |
|---|---|
| `01_DOMAIN_MODEL.md` | ownership del modelo y frontera de contratos compartidos. |
| `02_RUNTIME_AND_LIFECYCLE.md` | lifecycle y estado Engine. |
| `03_PERSISTENCE_AND_RECOVERY.md` | WAL/EFFECTIVE/recovery. |
| `04_CONCURRENCY_LEASES_AND_FENCING.md` | concurrencia y fencing. |
| `05_PROJECTION_AND_PUBLICATION.md` | CURRENT/FACTS. |
| `06_MANAGEMENT.md` | management/deactivation. |
| `07_CONFIGURATION_AND_MATERIALIZATION.md` | snapshot publicado, materialization y deuda pendiente. |
| `08_QUALIFICATION_BASELINE.md` | evidencia del gate actual. |
| `09_DECISION_INDEX.md` | decisiones/refinamientos vigentes en canonical. |
| `10_OPEN_ITEMS.md` | frontera abierta. |
| `12_COMMAND_CENTER_ANALYTICS_BOUNDARY.md` | FACTS vs Analytics. |
| `13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md` | adopción exacta. |

## Foco inmediato

No abrir un nuevo incremento del Engine ahora.

```text
NEXT
Command Center parity con ADA para Users / Profiles / Navigation / Manager

THEN
retomar y terminar el qualifier del cutover ada-contracts
```

Docker/distribución independiente de Engine/Delivery permanece **PLANNED / SEPARATE** después del cierre del gate actual.
