# Artifact and Distribution Boundary

Estado: **CURRENT — ADA BUILD/PRECHECK CLOSED AT a7546574 / DUAL-PRODUCT MANAGER ALIGNMENT NEXT**

Implementation:

```text
moragaga/atlanticus@a75465745e188da4765e803595b17acaa55d9306
```

Decisions:

```text
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

## CURRENT preservado de a7546574

El cierre de tooling anterior permanece válido:

```text
ADA starter thinning                    CLOSED
Tool-specific Home/pages/modules         CLOSED
project tooling extraction               CLOSED
.sh/.cmd human launcher contract          CLOSED
distribution build                       BUILT_UNQUALIFIED
internal wheels                          69
distribution precheck                    PRECHECK_PASS
source_git_head                          a75465745e188da4765e803595b17acaa55d9306
project.sh --help final artifact         PASS
```

No extender esos resultados a Docker/runtime/Azure que no fueron calificados.

## Frontera

```text
SOURCE PACKAGES
    ↓
PRODUCT COMPOSITION
    ↓
GENERATION / DISTRIBUTION TOOLING
    ↓
ARTIFACT
    ↓
HOST / DEVOPS / RUNTIME
```

Generated code no posee una segunda implementación de Manager.

## Manager authority

CURRENT source authority:

```text
package: atlanticus-web-manager
owner: web/capabilities/manager
version at a7546574: 0.3.18
```

No declarar como autoridad un pin copiado en:

```text
ADA Generic
Command Center
Starter
lock
manifest
wheelhouse
```

Esos son consumidores/materializaciones.

El próximo frente debe revisar y alinear todos ellos contra el package owner.

Si el contrato Manager necesita bump, primero se cambia el owner y después se regeneran consumers/artifacts.

## Dual-product convergence requirement

Antes de continuar qualification productiva de artifacts:

```text
1. converge reusable Manager compositions
2. ADA Generic consumes them
3. Command Center consumes them
4. ADA tooling/distribution aligns
5. Command Center tooling/distribution aligns
6. qualify both
```

El mismo chat ejecuta esta secuencia para evitar un período en que aplicaciones y artifacts tengan autoridades distintas.

## Command Center

No existe artifact final de Command Center generic acreditado.

No promover el host temporal Configuration Manager a Starter por conveniencia.

No utilizar el ZIP `ada_command_center_foundation_increment.zip`; quedó `SUPERSEDED / DO NOT APPLY`.

La futura distribución debe construirse a partir de la composición integrada real, no de un fixture ni de una composition paralela.

## Python e imagen

CURRENT observado:

```text
Python == 3.14.2
```

Target histórico:

```text
Python 3.14.7
python:3.14.7-slim-trixie
```

Estado de migración:

```text
BLOCKED / DEFERRED UNTIL EXPLICIT USER AUTHORIZATION
```

No modificar Python/base image durante Manager convergence o tooling alignment.

No usar esa diferencia como gate para bloquear el frente.

## OPEN posterior

Después de cerrar Manager dual-product:

```text
Command Center Resource Preparation/startup gate
final local runtime qualification
Docker qualification
Cosmos/Azurite physical integration
Entra/Azure production
```

No abrir esos frentes antes del cierre del contrato Manager compartido.
