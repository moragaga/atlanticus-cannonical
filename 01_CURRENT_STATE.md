# Atlanticus — Current State

Estado: **CURRENT — DUAL APP DURABLE + MASTER PROJECTION CLOSED; SOURCE CONVERGENCE NEXT**

## Autoridad

```text
Implementation evidence checkpoint
moragaga/atlanticus@7bd11afdf2af82c56fb100f4aa5336c039d9bd22

Decisions
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e

Canonical inspected before replacement
moragaga/atlanticus-cannonical@cedbe3156bc384add0de58cda27f2b77628c133d
```

Git permanece **SOLO LECTURA**.

## Estado resumido

```text
WEB-DISTRIBUTION-SHARED-ENGINE-CLEANUP          CLOSED / CURRENT
DUAL-APP-ENV-DETAIL-CONTRACT                    CLOSED / CURRENT
COMMAND-CENTER-DURABLE-RUNTIME-COMPOSITION      CLOSED / VERIFIED
ATLANTICUS-WEB-MASTER-PROJECTION                CLOSED / VERIFIED / CURRENT
DUAL-APP-MASTER-PROJECTION-CONVERGENCE          CLOSED / VERIFIED

SOURCE-NAMESPACE-AND-COMPOSITION-CONVERGENCE    PLANNED / NEXT
DUAL-APP-DURABLE-RUNTIME-SMOKE                  PLANNED / AFTER SOURCE
SCOPE-TOOLING-TOPOLOGY-NORMALIZATION            PLANNED / DEFERRED
ADA-KPI-COLLECTOR-OPERATIONAL-E2E               PLANNED / AFTER APP SMOKE
ADA-UI-RECONSTRUCTION                           PLANNED / AFTER COLLECTOR
```

## Master Projection — CURRENT

Motor reusable:

```text
atlanticus-web-master-projection==0.1.0
web/capabilities/master-projection
```

Responsabilidades del motor:

```text
material
reader
plan
apply
independent Web surface
```

ADA y Command Center conservan sólo composición/provisioning de producto.

## ADA Generic — CURRENT

```text
ada-generic-application==0.2.22
Python == 3.14.2
```

ADA Generic consume `atlanticus-web-master-projection==0.1.0`.

Master material continúa derivado del namespace de aplicación:

```text
<application-namespace>/master-projection/material.zip
```

## ADA Command Center — CURRENT

```text
ada-command-center-generic-application==0.1.1
ada-command-center-configuration-manager==0.1.2
```

El host local admite:

```text
ADA_MANAGER_PERSISTENCE_PROVIDER=local
ADA_MANAGER_PERSISTENCE_PROVIDER=durable
```

`ATLANTICUS_ENVIRONMENT=local` controla host/identity behavior.
El provider controla persistencia. Emulator versus Azure es configuración de conexión, no modo
arquitectónico.

Durable Command Center compone:

```text
Blob Source
Cosmos Alarm Configuration Projection
Cosmos Profiles Projection
Cosmos Navigation Projection
Blob Users Registry
Cosmos Users Runtime
```

Master Projection Command Center usa el motor genérico y sus dominios:

```text
Profiles
Navigation
Alarm Configuration
```

Material derivado:

```text
conciencia_situacional/command-center/master-projection/material.zip
```

## Gap transversal CURRENT

Command Center aún importa:

```text
ada.web.storage.namespace.AdaStorageNamespace
```

Eso hace que un producto dependa de una capability ubicada bajo el scope ADA para resolver un
contrato que ya es reutilizado por ambos productos.

El próximo incremento no reescribe `SourceStore`; corrige esta frontera de namespace/composición
y alinea el consumo de Source entre ADA y Command Center.

## Qualification de este cierre

Evidencia local reportada:

```text
Command Center Configuration Manager     31 passed
Command Center Generic                   10 passed
ADA Generic                              249 passed
atlanticus-web-master-projection         56 passed

Ruff / format en los tres paquetes del cierre    PASS
ADA commented host AST mirror                    PASS
git diff --check                                  PASS
```

## UNVERIFIED

```text
actual dual-app durable connection smoke
resource preparation parity for Command Center
current-head Web distribution artifacts
Docker/image runtime from current packages
Azure/Entra production
KPI/Collector/UI E2E
```
