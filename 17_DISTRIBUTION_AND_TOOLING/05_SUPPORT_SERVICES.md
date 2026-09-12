# Supporting Services in Distribution

Estado: **CURRENT DIRECTION**

## Propósito

Un distributable puede declarar servicios de apoyo requeridos.

Los principales caballos de batalla iniciales son:

- Cosmos;
- Storage.

## No acoplar infraestructura productiva al artifact

El artifact no "crea Azure".

Declara:

```text
SupportServiceRequirement
├── service kind
├── logical connection name
├── local strategy
├── cloud expectation
└── resource requirements
```

## Local

La distribución local puede levantar servicios equivalentes cuando corresponda:

```text
Docker Compose
├── application/process
├── Cosmos emulator / compatible local service
└── Storage/Azurite
```

si la capability lo soporta.

## Cloud

La infraestructura base permanece externa:

```text
Support / IaC
→ Cosmos account/database
→ Storage account
```

La Web/bootstrap prepara sólo recursos de aplicación permitidos.

## Beneficio

El mismo distributable declara lo que necesita sin:

- hardcodear Azure DevOps;
- pedir que cada job cree infraestructura;
- ocultar dependencias externas.
