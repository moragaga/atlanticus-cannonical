# Source Storage — Implementation Order

Estado: **CURRENT PLAN**

## Checkpoint

```text
SOURCE-1A.1                       Core + Local                  CLOSED / VERIFIED
SOURCE-1A.2                       Blob                          CLOSED / VERIFIED
Projection                        Exact-release Core            CLOSED / VERIFIED
USERS-CANONICAL-PROJECTION-2      Users/Cosmos provider         CLOSED / VERIFIED
MANAGER-ROOT-CANONICAL-CUTOVER    Root Projection transport     CLOSED / VERIFIED
USERS-EXACT-MANAGER-LIFECYCLE     Users Manager consumer        CLOSED / VERIFIED
```

## Orden cerrado

1. Source audit por dominio — CLOSED.
2. Functional Source manifest — CLOSED.
3. Release Model — CLOSED.
4. `SourceStore` — CLOSED.
5. Concurrency/current promotion — CLOSED.
6. Local provider — CLOSED.
7. Core + Local qualification — CLOSED.
8. Blob provider + Azurite qualification — CLOSED.
9. Projection exact-release Core — CLOSED.
10. Users canonical Projection/Cosmos — CLOSED.
11. Manager root exact-target transport — CLOSED.
12. Users Manager exact Source/Projection/History lifecycle — CLOSED.

## Step 12 — Users Manager exact lifecycle

Current:

```text
validate        EXACT
read            EXACT
publish         EXACT
status          EXACT
project         EXACT
history         EXACT
legacy workflow NONE
```

Esto cierra el consumer Manager Users, no todos los consumers legacy Users del repositorio.

## Próximas fronteras de Source/Projection

### Users runtime exact provenance

```text
USERS-RUNTIME-EXACT-RELEASE-PROVENANCE
PLANNED
```

- reemplazar provenance runtime legacy;
- no equiparar `UsersConfigurationBundle.revision` con `SourceReleaseId`;
- no introducir shim release/string;
- conservar CAS/ETag del writer runtime.

### Users runtime canonical cutover

```text
USERS-RUNTIME-CANONICAL-CUTOVER
PLANNED
```

### Consumer migrations restantes

PLANNED:

- Navigation administrative migration;
- otros consumers Users legacy fuera del Manager ya migrado;
- Tools/KPI/KPI Definitions cuando corresponda.

## Blocker transversal actualmente visible

El full ADA suite expone un gap del contrato Projection legacy en:

- Navigation;
- Tools;
- KPI;
- KPI Definitions.

Ese frente es:

```text
ADA-LEGACY-PROJECTION-CONTRACT-ALIGNMENT
PLANNED / NEXT PROJECT FOCUS
```

No forma parte de Source Core ni reabre Users exact lifecycle.

## Legacy retirement

Sólo después de demostrar ausencia de consumers productivos:

- retirar bindings Source legacy;
- retirar contracts legacy;
- retirar SharePoint/Power Automate donde corresponda.

## Regla de reemplazo

Cuando un incremento sustituya implementación existente, entregar:

```text
DELETE
KEEP
MODIFY/REPLACE
GATES AFTER DELETE
```

No dejar legacy temporal por defecto.

## Regla de UI

No diseñar UI antes de congelar el contrato backend que consume.

## Regla de chat/checkpoint

Cuando un step queda CLOSED / VERIFIED:

1. actualizar canonical;
2. validar;
3. cerrar el foco;
4. abrir chat nuevo para el siguiente step.
