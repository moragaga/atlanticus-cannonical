# Frontend Generation

Estado: **CURRENT DIRECTION**

## Objetivo

Aplicar al frontend el mismo principio que al backend:

```text
ADA Generic
    ↓
Tool Configuration / Composition
    ↓
Generated Web Application
    ↓
qualification
    ↓
Distributable Web Artifact
```

## Output

La aplicación generada debe quedar **lista para ser distribuida**, no sólo como código de ejemplo.

Debe materializar según contrato:

- application package;
- shell;
- branding;
- navigation;
- configuration bindings;
- runtime experience;
- loaders;
- assets;
- Entra integration;
- health/readiness/bootstrap surface;
- env.detail;
- dependency lock;
- tests/gates;
- distribution metadata.

## Tool-specific behavior

ADA Generic genera/compone la base.

Comportamiento realmente específico puede agregarse después sin modificar el núcleo genérico.

## DevOps

Igual que backend:

```text
Atlanticus
→ entrega artifact distribuible

DevOps
→ decide cómo su pipeline corporativo lo despliega
```

No acoplar el generador Web a una implementación particular de Azure DevOps.
