# Manager — ADA Component External Links

Estado: **CURRENT DIRECTION / CONTRACT DESIGN**

## Objetivo

Dentro del apartado ADA del Manager, cada Component puede exponer links externos asociados.

En UI aparecen junto al nombre del Component mediante un popover.

## No hardcodear links en layout

Crear una configuración/catálogo separado, conceptualmente:

```text
ComponentExternalLinks
├── tool_key
├── component_key
└── links[]
    ├── link_key
    ├── label
    ├── url
    ├── enabled
    └── order
```

Shape final pendiente.

## Identity

La referencia debe usar las identidades canónicas de Tool/Component.

No crear un segundo identificador visual arbitrario si `component_key` ya resuelve la identidad.

Si aparece una necesidad adicional de lookup, se define explícitamente.

## JS controlled popover

El popover debe estar controlado del lado cliente/JS cuando corresponda para que:

- interval callbacks;
- actualizaciones de datos;
- rerenders operacionales;

no lo cierren accidentalmente.

La interacción UI no debe depender de que el servidor conserve el estado visual del popover.

## Warmup

El catálogo de links es pequeño y de lectura frecuente.

Puede participar del warmup de configuración:

```text
startup
→ load active Component link catalog
→ cache/runtime state
→ fast client access
```

No realizar lectura remota completa en cada apertura del popover.

## Boundary

Esta configuración pertenece a experiencia/metadata de la Tool.

No debe mezclarse con:

- Alarm Configuration;
- KPI data;
- Operational Data.

Puede consumir Tool/Component identity como reference.
