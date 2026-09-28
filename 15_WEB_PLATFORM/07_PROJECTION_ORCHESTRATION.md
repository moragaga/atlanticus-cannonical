# Web Platform — Projection Orchestration

Estado: **CURRENT CONTRACT / USERS SPECIAL RECOVERY IMPLEMENTED / MASTER PROJECTION PLANNED**  
Inspección: `atlanticus@208c8d6244795ba92cbe6f8e6b11e9743191367d`; canonical base `e49901fb3ceef5431edbde1d0dcbb29fc3502855`.

## Contrato ordinario CURRENT

No introducir un orden global artificial entre todas las proyecciones. Las dependencias semánticas que forman parte de la identidad exacta se declaran mediante `ProjectionTarget.dependencies`, conservando `ProjectionTarget` completos y normalizados. Las dependencias de mera lectura son resoluciones derivadas; no inventar ciclos entre Sources.

Ejemplos de Sources independientes cuando sus contratos lo permiten:

```text
Navigation Source -> Navigation Projection
Tool Source       -> Tool Projection
```

KPI Configuration conserva el `Tool ProjectionTarget` exacto de su dependencia. KPI Definition y cualquier otro dominio deben usar sus contratos implementados al ejecutarse; una revision string privada no sustituye un target dependiente. Reintentar un mismo target exacto no debe crear artificialmente una nueva identidad funcional.

## Users es una excepción administrativa CURRENT

La capability de Users dispone de:

```text
UsersAdministrationService: discover / promote / update
UsersApprovedRecoveryService: preview_capture / capture / validate / restore
                              validate_replace / replace_approved
```

`UsersApprovedRecoveryService` reconstruye desde **snapshots expresamente aprobados**; el registro durable puede contener candidatos. `validate_replace` ofrece un plan y `replace_approved` reconcilia Blob/Cosmos con auditoría y respaldo previo. Esta operación no es un `ManagerModule` Source/Projection sintético ni un target ordinario del Coordinator.

La página interna **Proyección de usuarios** es `ManagerEntry` autorizada con `users.manage`. Contiene **Crear respaldo** y **Proyectar usuarios**; el operador compara antes de escoger restauración estricta o sustitución completa. No hace bootstrap del Manager ni garantiza por sí misma aislamiento/revocación productivos.

## Master Projection — próxima extensión, no implementada

La página externa usará los proyectores actuales y el proceso especial de Users donde aplique. No editará Sources ni inventará otro sistema de publicación. Ante ausencia de material protegido de acceso, mostrará página controlada sin permitir interacción privilegiada; con material válido y autenticación, expondrá estado, plan y acciones autorizadas para las proyecciones necesarias del ambiente.

- Derivar orden sólo de dependencias reales y precondiciones actuales; no fijar una lista rígida por comodidad de UI.
- Verificar Sources disponibles y distinguir proyecciones ya alineadas de faltantes/desactualizadas cuando el contrato de cada dominio lo permita.
- Exponer resultados por componente y reintentos controlados; sin fingir transacción global Blob/Cosmos.
- Resolver explícitamente la fuente aprobada y `identity_realm` de Users en destinos **sin promovidos**, porque el provider durable actual del Manager no opera allí.
- Definir antes de implementar contrato de credenciales, archivo y límites de autorización independientes del Manager.

## Identificación operacional ADA — otro foco

Cargo, área y grupo de ADA no son campos de Atlanticus Users y no otorgan permisos. Su Source/Projection propio no forma parte de Master hasta que el contrato correspondiente exista y su integración se autorice. No mezclar ese desarrollo con Master.

## Estado

```text
EXACT PROJECTION TARGET DEPENDENCIES           CURRENT
USERS SPECIAL VALIDATE / RESTORE / REPLACE     CURRENT / LAB VALIDATED
MANAGER USERS PROJECTION ENTRY                CURRENT / USER-REPORTED UI VALIDATED
ISOLATED MASTER PROJECTION                    PLANNED / UNVERIFIED
MASTER FILE / WARMUP TOOLING                  PLANNED / CONTRACT OPEN
```
