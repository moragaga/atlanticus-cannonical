# ADA Command Center — Web Application

Estado: **CURRENT — TEMPORARY MANAGER HOST AVAILABLE / GENERIC PRODUCT HOST NOT IMPLEMENTED / MANAGER CONVERGENCE CLOSED LOCALLY / GENERIC COMPOSITION NEXT**

## Authority of this close

```text
Last confirmed Atlanticus HEAD:
moragaga/atlanticus@36361dd570f86e8350ea4a6ee0e09bab351ba171

Command Center Manager convergence delta:
VERIFIED LOCAL / PENDING FINAL GIT HEAD
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

No equivale a la aplicación final de Command Center.

No existe implementación acreditada de `ada-command-center-generic-application`.

## Composition administrativa CURRENT

El host actual no duplica las capabilities.

Usa:

```text
compose_alarm_configuration_manager(...)
create_tool_catalog_manager_entry(...)
    ↓
ManagerSurfaceDefinition
```

Estado:

```text
Alarm Configuration Manager composition      CURRENT / VERIFIED
Tool Catalog Manager composition              CURRENT / VERIFIED
Command Center Configuration Manager host     CURRENT / TEMPORARY
```

Convergencia local calificada:

```text
ada-command-center-web-alarm-configuration==0.1.1
ada-command-center-web-tool-catalog-manager==0.1.1
ada-command-center-configuration-manager==0.1.1
atlanticus-web-manager==0.3.19
```

Qualification local:

```text
Alarm Configuration       124 PASS
Tool Catalog Manager        9 PASS
Configuration Manager host 28 PASS
Ruff                        PASS
git diff --check            PASS
Manager 0.3.18 rg           EMPTY
```

La delta queda pendiente únicamente de un HEAD Git final si todavía no fue integrada.

## Alarm workspace convergence

Alarm Configuration conserva una especialización real:

```text
Save Draft
→ require Confirmed Tool Catalog
→ pin _confirmed_tool_catalog_revision
```

La mecánica genérica de workspace ya no se duplica:

```text
owner
SourceKey
SourceSnapshot
workspace serialization/validation
    ↓
ManagerWorkspaceBinding
```

No hay wrapper de compatibilidad para comportamiento antiguo.

## Futuro host integrado

El futuro Command Center Generic/Application no debe reconstruir manualmente estas capabilities.

Dirección:

```text
existing product capabilities
    ↓
Command Center product composition
    ↓
integrated WebApplicationDefinition / runtime
```

El host standalone se conserva como superficie de desarrollo/qualification mientras siga siendo útil, pero no constituye un servicio remoto obligatorio.

Su retiro o absorción sólo puede decidirse después de que el host genérico cubra y califique sus responsabilidades.

## Users / Profiles / Navigation

Command Center todavía no integra estas capabilities.

No copiar el wiring interno de ADA.

Las reusable compositions existen y están convergidas, pero eso no constituye una necesidad de producto:

```text
Users Manager composition      CURRENT / AVAILABLE
Profiles Manager composition   CURRENT / AVAILABLE
Navigation Manager composition CURRENT / AVAILABLE
```

El próximo diseño debe decidir si el composition root real de Command Center requiere alguna de ellas.

## Generic application — NEXT

El siguiente chat debe definir primero, sin implementación prematura:

```text
application boundary
composition root
entrypoint
runtime ownership
Manager integration
Home mínima sólo si existe necesidad real
providers/stores realmente requeridos
responsabilidad restante del temporary host
qualification gate
```

No inferir Navigation, Users, Profiles, Live, dashboard, History o Analytics por simetría con ADA.

## Sequence

La secuencia anterior:

```text
Manager convergence
→ ADA
→ Command Center
→ tooling/distribution dual
```

queda refinada:

```text
Manager convergence reusable                         CLOSED
ADA Generic integration                              CLOSED
Command Center existing Manager components alignment CLOSED locally
Command Center Generic Application                   NEXT
dual-product tooling/distribution                    AFTER GENERIC APP
Resource Preparation / startup gate                  LATER / SEPARATE
```

## Invariants

- Command Center Web es producto propio; no se monta como página de ADA Generic.
- Reutilización de Atlanticus no convierte una frontera lógica en servicio remoto.
- Manager y future Command Center operational shell conservan ownership separado.
- ADA Access no pertenece a Command Center.
- No crear un segundo sistema de identidad.
- No inventar contratos Live/Analytics para levantar el host.
- No copiar demo/fixtures como producto.
- No introducir aliases, shims o legacy para conservar compositions reemplazadas.
- Contratos antes que consumidores.
- Backend antes que frontend cuando se abra una nueva frontera real.

## Python

Los paquetes Web relevantes continúan declarando Python `3.14.2`.

Migración 3.14.7/Trixie:

```text
BLOCKED / DEFERRED UNTIL EXPLICIT USER AUTHORIZATION
```
