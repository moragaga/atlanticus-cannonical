# Frontend Generation

Estado: **CURRENT DIRECTION / WEB ARTIFACT QUALIFICATION OPEN**

Inspección del delta: `moragaga/atlanticus@a6061ffed59c8b04e64b0a7fdc17050ef463c850`.

## Objetivo conservado

```text
ADA Generic
→ Tool Configuration / Composition
→ Generated / Packaged Web Application
→ qualification
→ Distributable Web Artifact
```

ADA Generic ya ejecuta su base y consumió Navigation publicada/proyectada en local.
Este resultado no demuestra todavía que el artifact Web sea transportable fuera del
checkout ni que una Tool real esté configurada.

## Output deseado por el contrato existente

Según necesidad y capacidades efectivamente utilizadas:

- application package + entrypoint;
- shell, branding y Navigation;
- configuration/Tool projection bindings;
- runtime experience, assets y loaders;
- host productivo de identidad (Entra/Graph, pendiente);
- readiness/bootstrap/error surfaces;
- `.env.detail` sin secretos y dependencia lock;
- qualification/tests y metadatos de distribución.

No afirmar que estos puntos ya están todos implementados o verificados por el smoke local.
No crear generador nuevo antes de inspeccionar paquetes/scripts Web existentes.

## Invariantes

La app genérica termina en estado operacional por `dcc.Store`/ToolComponent; el render
particular lo aporta cada Tool sin contaminar el núcleo. El Collector existente se
reutiliza, no se clona.

## Próxima qualification — único foco

`ADA-GENERIC-WEB-ARTIFACT-DISTRIBUTION-QUALIFICATION`:
reconciliar tooling Web actual, elaborar matriz de archivos/dependencias de salida y
probar instalación/ejecución transportable. Cambios de código sólo ante gap comprobado
y nuevo consenso. El bundler de procesos backend tiene ownership propio.

Atlanticus entrega artifact y contrato; DevOps ejecuta el pipeline externo.
