# ADA Command Center — Tool to Alarm Configuration

Estado: **CURRENT — exact Tool snapshot + strict routing; shared Tool contracts migrated to `ada-contracts-tools`**.

## Ownership CURRENT

Tool Configuration posee identidad, kind, estructura y relaciones de cada Tool.

Alarm Configuration publica referencias y evidencia congelada sin redefinir Tool Catalog.

Shared contract owner:

```text
ada-contracts-tools==1.0.0
    ToolConfigurationKind / ToolScope
    ToolStructure
    Tool source contracts
    ToolDependencyEntry
    ToolDependencyManifest
```

`scopes/ada-command-center/domain/tools` está **SUPERSEDED / REMOVED**.

## Tool services Web CURRENT

```text
web/tools/catalog
web/tools/discovery-cosmos
web/tools/catalog-manager
```

El catálogo consolidado usa tipos de `ada.contracts.tools`. Cuando consume `ada.web.tools.configuration.ToolConfiguration`, convierte explícitamente kind/structure al contrato neutral antes de almacenar `ToolCatalogEntry`.

## Publish exact Rn/Cn CURRENT

```text
Confirmed Tool Catalog Cn
→ authoring/validation
→ AlarmConfigurationSnapshot(Rn, ToolDependencyManifest(Cn))
```

El manifest forma parte del snapshot publicado y conserva evidencia exacta del Tool Catalog usado en esa publicación.

## Routing policy CURRENT

Owner específico de Command Center:

```text
ada-command-center/domain/alarms/routing_policy.py
```

Policy:

```text
PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC -> END
```

La policy consume `ToolConfigurationKind` desde `ada-contracts-tools`.

## Frontera materialization

DECIDED target:

```text
Command Center valida/resuelve Tool references antes de publicar
Materialization consume snapshot válido
Materialization no vuelve a descubrir Tools ni revalidar semántica Tool
```

La implementación CURRENT aún conserva deuda downstream; no elevar esa deuda a contrato.

## Visual boundary

- Tool keys/component/subcomponent keys son datos contractuales.
- geometría/layout/CSS pertenecen a Web.
- STRATEGIC puede participar en routing sin implicar automáticamente contrato visual.

No inferir que visual targets y routing assignments son el mismo hecho.
