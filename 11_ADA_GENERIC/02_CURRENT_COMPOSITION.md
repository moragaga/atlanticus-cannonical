# ADA Generic — Current Composition

Estado: **CURRENT — PRODUCT RUNTIME AUTHORITY + MASTER PROJECTION OWNERSHIP**

## Version CURRENT

```text
ada-generic-application==0.2.21
Python == 3.14.2
```

## Composition root

ADA Generic es el host integrado de ADA.

Posee:

```text
settings
local/durable Manager composition
identity binding
Tool Projection resolution
operational render binding
Master Projection
KPI Collector attachment
Web runtime lifecycle
```

El standalone Configuration Manager permanece como aplicación separada de desarrollo/qualification; no es un servicio remoto requerido.

## Host boundary

El runtime reusable vive en el product package.

El Starter ADA sólo delega y aporta extensiones explícitas de host/composition.

Invariante:

```text
starter
-X-> duplicate persistence selection
-X-> duplicate Manager lifecycle
-X-> duplicate Master Projection reader
-X-> duplicate worker lifecycle
```

## Master Projection — CURRENT

Runtime ownership:

```text
ada.web.application.generic.master_projection
```

Incluye material/reader/provisioning más plan/composition/apply/web.

Local resource preparation también pertenece a ADA Generic.

Master Projection es una extensión/runtime capability, no una aplicación ni tooling.

## Persistence modes

```text
ADA_PERSISTENCE_MODE=local
→ local Source / Projection / Manager
→ local Master Projection path

ADA_PERSISTENCE_MODE=durable
→ Blob Source / Manager / Master Projection
→ Cosmos Projection / Manager
```

El entorno puede seguir siendo `local` mientras persistence es `durable`, permitiendo Storage real con Cosmos local/emulado.

## Content State authoring

La application definition soporta:

```text
ContentStatePresentationMode.NORMAL
ContentStatePresentationMode.AUTHORING
```

`AUTHORING` suprime overlays visuales degradados pero no falsifica el estado runtime.

No hay aún una variable `.env.detail` congelada para seleccionarlo.

## Distribution

ADA distribution:

```text
starter            PASS
internal wheels    71
qualification      PRECHECK_PASS
runtime/image      UNVERIFIED
```

No promover este resultado a runtime qualification.
