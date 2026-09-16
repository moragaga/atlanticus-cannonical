# ADA Generic — Scope

Estado: **CURRENT**

## Propósito

ADA Generic compone la base reutilizable necesaria para materializar una ADA.

No debe convertirse en una megaaplicación ni absorber cada regla de negocio de las capabilities ADA-specific.

## Frontera de ownership

Atlanticus aporta infraestructura y capacidades genéricas.

ADA puede poseer capabilities propias bajo `scopes/ada` y consumir esos contratos genéricos directamente.

Ejemplos de capabilities ADA-specific:

```text
Tools
KPI Configuration
KPI Definition
future Access
```

Estas capabilities no pasan a ser core genérico por usar:

```text
SourceStore
ProjectionStore
SourceProjectionService
Manager
Navigation
Profiles
Users
```

## Alcance de composición

ADA Generic puede integrar/resolver desde configuración:

- identidad de Tool;
- Tool Structure;
- branding;
- navegación;
- sources/participación;
- Component/Collector contracts;
- KPI configuration/definition;
- alarm configuration/projection;
- render binding;
- runtime experience;
- demás contratos configurables aprobados.

Integrar no significa adquirir ownership del dominio ni moverlo al core Atlanticus.

## Handoff

Una vez resuelta la base/configuración, el usuario puede continuar construyendo o generando el código específico de su ADA sobre contratos estables.

ADA Generic habilita autoservicio/composición.

No sustituye el código específico cuando una Tool requiere comportamiento propio legítimo.

## Regla canónica

```text
GENERIC INFRASTRUCTURE
reusable across products

ADA-SPECIFIC DOMAIN
remains under scopes/ada

COMPOSITION
explicitly connects both
```

El núcleo genérico de Atlanticus nunca depende de ADA.
