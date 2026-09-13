# Web Platform — Resource Provisioning

Estado: **DIRECTION FROZEN / RESOURCE INVENTORY PENDING**

## Principio

Web es el orquestador de preparación de recursos de aplicación.

Backend no debe provisionar/verificar infraestructura en cada ejecución de job.

## Pero los containers NO se congelan todavía

Antes de definir todos los:

- Cosmos containers;
- partition keys;
- TTL;
- Storage containers;
- paths;

debemos completar la traza real de componentes usados por cada Tool y aplicación.

Orden:

```text
Tool/Application
→ Components
→ capabilities/runtimes
→ persistence/projections
→ resource requirements
→ container inventory
```

## Razón

Congelar containers antes de conocer toda la composición puede producir:

- containers innecesarios;
- particiones incorrectas;
- duplicación de authorities;
- acoplamiento prematuro.

## Baseline conocido

Cosmos ya dispone de provisioning/validation contracts.

Web Storage Topology y el bridge provider-specific Cosmos están implementados y validados:

```text
WEB-STORAGE-TOPOLOGY              CLOSED / VERIFIED / CURRENT
USERS-STORAGE-TOPOLOGY            CLOSED / VERIFIED / CURRENT
STORAGE-PREFLIGHT-COSMOS-BRIDGE   CLOSED / VERIFIED / CURRENT
```

El bridge:
- traduce `ResolvedStoragePlan` a `CosmosContainerSpec`;
- usa provisioners preconstruidos por `connection_ref`;
- ejecuta `ensure_containers` o `validate_containers` según la operación solicitada;
- no crea database;
- no contiene secretos ni Azure SDK;
- no define lifecycle/readiness de aplicación.

`users.runtime` es el primer recurso durable confirmado individualmente, pero no cierra la inventory global.

Storage requiere cerrar su parity de provisioning.

## Local

Cuando el lifecycle/plan de aplicación esté congelado:

```text
Web
→ puede crear DB local según política explícita
→ ensure resources
```

La creación local de database no pertenece al bridge Cosmos de Storage.

## Cloud

```text
base account/database preexistente
→ Web validates/ensures allowed application resources
```

## Mismatch

Partition key/TTL incompatible sigue siendo error contractual.

No auto-mutación silenciosa.

## Estado

`ApplicationResourcePlan` permanece abierto hasta cerrar la traza de:

1. Operaciones Integradas;
2. Mina;
3. Manager;
4. Command Center;
5. KPI pipeline;
6. Alarm pipeline;
7. User Activity;
8. Source/Blob.

También permanecen OPEN:
- owner/required/optional semantics;
- named connection resolution global;
- permisos Cloud para resource creation;
- READY/DEGRADED/ERROR y wiring de readiness;
- punto de composición/lifecycle Web que invoque el bridge.
