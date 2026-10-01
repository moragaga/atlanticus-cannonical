# ADA Command Center — Web Application

Estado: **CURRENT — TEMPORARY MANAGER HOST AVAILABLE / GENERIC PRODUCT HOST NOT IMPLEMENTED / MANAGER CONVERGENCE NEXT**

Implementation:

```text
moragaga/atlanticus@a75465745e188da4765e803595b17acaa55d9306
```

## Capabilities Web CURRENT

```text
scopes/ada-command-center/web/
  alarms/configuration
  alarms/persistence
  alarms/projection-local
  alarms/projection-cosmos
  tools/catalog
  tools/discovery-cosmos
  tools/catalog-manager
  application/ada-command-center-configuration-manager
```

El Configuration Manager existente es un host temporal/standalone.

No equivale todavía a la aplicación final de Command Center.

## Composition administrativa CURRENT

El host actual no duplica las capabilities.

Usa:

```text
compose_alarm_configuration_manager(...)
create_tool_catalog_manager_entry(...)
    ↓
ManagerSurfaceDefinition
```

Estado auditado:

```text
Alarm Configuration Manager composition
CURRENT / VERIFIED

Tool Catalog Manager composition
CURRENT / VERIFIED

Command Center Configuration Manager host
CURRENT / TEMPORARY HOST
```

El principal local quedó convergido con Manager Core:

```text
profile_keys=('local',)
access_keys=()
administrative_override=True
is_local=True
```

Tool Catalog usa `manager_access_granted(...)`.
Alarm Configuration usa la policy genérica `can_view(...)`.

## Futuro host integrado

El futuro Command Center Generic/Application no debe reconstruir manualmente estas capabilities.

Dirección congelada:

```text
capabilities
    ↓
Command Center configuration composition
    ↓
ManagerSurface
    ↓
Command Center integrated host
```

El host standalone se conserva como superficie de desarrollo/qualification mientras siga siendo útil, pero no constituye un servicio remoto obligatorio.

## Users / Profiles / Navigation

Command Center todavía no integra estas capabilities.

No copiar el wiring interno de ADA.

La regla ahora es:

```text
Users Manager composition      → reutilizar Atlanticus
Profiles Manager composition   → reutilizar Atlanticus
Navigation Manager composition → BLOCKED until convergence
```

Navigation debe ser convergida primero en Atlanticus y adoptada por ADA; sólo después Command Center consume esa misma composition.

## Starter / Generic application

No existe implementación acreditada de `ada-command-center-generic-application`.

El ZIP propuesto durante la auditoría de este chat:

```text
ada_command_center_foundation_increment.zip
```

queda:

```text
SUPERSEDED / DO NOT APPLY
```

No forma parte de Git ni de canonical CURRENT.

## Secuencia anterior SUPERSEDED

La planificación anterior declaraba:

```text
Resource Preparation + startup gate → NEXT
Starter runtime/Home → después
Users/Profiles/Navigation → excluidos inicialmente
```

La decisión explícita de este cierre reemplaza ese orden para la frontera Manager.

Nueva secuencia:

```text
1. Manager composition/version convergence
2. integrate same Manager authority into ADA Generic
3. integrate same Manager authority into Command Center
4. align tooling/distribution of both products
5. qualification
6. only then resume Resource Preparation / startup gate
```

Resource Preparation sigue siendo importante pero deja de ser el siguiente foco.

## Invariantes

- Command Center Web es producto propio; no se monta como página de ADA Generic.
- Reutilización de Atlanticus no convierte una frontera lógica en servicio remoto.
- Manager, Operational Navigation y future Command Center shell conservan ownership separado.
- ADA Access no pertenece a Command Center.
- No crear un segundo sistema de identidad.
- No inventar contratos Live/Analytics para levantar el host.
- No copiar demo/fixtures de prueba como producto.
- No introducir aliases o legacy para conservar compositions reemplazadas.

## Python

Python CURRENT de los paquetes relevantes permanece `3.14.2`.

Migración 3.14.7/Trixie:

```text
BLOCKED / DEFERRED UNTIL EXPLICIT USER AUTHORIZATION
```
