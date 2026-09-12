# env.detail Contract

Estado: **CURRENT DIRECTION**

## Problema actual

`env.detail` existe, pero muchos archivos contienen sólo:

```text
KEY=value
```

sin explicar:

- qué significa;
- por qué existe;
- qué valores acepta;
- si es sensible;
- quién lo consume.

## Objetivo

Cada proceso/aplicación estable debe tener un `env.detail` verdaderamente documental.

Debe permitir entender cada variable sin leer el código.

## Información mínima por variable

Para cada entrada documentar:

- key;
- purpose;
- required/optional;
- accepted values/format;
- example no sensible;
- sensitive yes/no;
- source esperado;
- razón arquitectónica;
- owner/consumer.

El formato final puede seguir siendo simple y humano; no convertirlo en un schema innecesariamente complejo.

## Producción

`env.detail` **NO contiene secretos**.

Es referencia/contrato.

El runtime productivo continúa usando los mecanismos acordados de configuración/secret resolution.

## Ejemplo conceptual

```text
PI_SOURCE=NOTPII
# purpose: selects PI source provider
# accepted: NOTPII | PI_WEB_API
# required: yes
# sensitive: no
# reason: runtime must select exactly one PI adapter
```

## Current evidence

El `kpi-runtime/.env.detail` actual contiene variables reales pero todavía sin estas explicaciones.

Eso queda como deuda documental a cerrar durante productización.
