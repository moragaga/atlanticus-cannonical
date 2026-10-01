# Frontend Generation

Estado: **CURRENT / ADA DISTRIBUTION TOOLING CLOSED FOR a7546574 / MANAGER ALIGNMENT NEXT**

Implementation:

```text
moragaga/atlanticus@a75465745e188da4765e803595b17acaa55d9306
```

## Frontera

Atlanticus genera Starters/artifacts editables y reutilizables.

No debe copiar una segunda implementación completa del runtime, Manager o project tooling dentro de cada aplicación.

```text
capabilities/packages
        ↓
product composition
        ↓
generation/distribution tooling
        ↓
generated application/artifact
```

## ADA CURRENT

El perfil ADA usa ADA Generic como composition/runtime authority.

La Tool generada conserva sus propias:

```text
application.pages
application.modules
Home
host/deployment surfaces
```

El project tooling reusable vive en:

```text
tooling/distribution/web/ada/project-tooling
```

y se distribuye como:

```text
ada-project-tooling==0.1.0
```

## Manager authority in generated artifacts

La generación no debe fijar una versión Manager diferente de la autoridad del package owner.

CURRENT package owner:

```text
web/capabilities/manager
atlanticus-web-manager==0.3.18
```

El próximo frente debe revisar todos los pins/locks/manifests/wheels que materialicen Manager.

Si el package owner aumenta de versión durante la convergencia, el tooling debe regenerar y calificar los artifacts con esa misma versión.

No mantener artifacts ADA y Command Center con autoridades Manager divergentes.

## Command Center tooling

Command Center todavía no posee una aplicación generic/distribución final acreditada.

No inventar un segundo framework de generación.

Después de integrar el Manager convergido en Command Center, el mismo frente debe alinear su generación/distribución usando los contratos de tooling ya existentes donde correspondan.

La implementación exacta se deriva del código existente en ese momento; no crear adapters o scripts duplicados por adelantado.

## Python / image freeze

Python CURRENT:

```text
3.14.2
```

Migración 3.14.7 / Trixie:

```text
BLOCKED / DEFERRED UNTIL EXPLICIT USER AUTHORIZATION
```

No cambiarla como parte de Manager/tooling alignment.

## Próximo foco

Tooling no es el primer paso independiente.

Secuencia obligatoria:

```text
Manager composition/version convergence
→ ADA Generic integration
→ Command Center integration
→ tooling/distribution alignment of both
→ qualification
```

Todo pertenece al mismo siguiente frente para evitar que artifacts y aplicaciones vuelvan a divergir.
