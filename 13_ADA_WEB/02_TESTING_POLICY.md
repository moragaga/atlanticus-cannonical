# ADA Web — Testing Policy

Estado: **CURRENT**

## Objetivo

Los tests Web protegen comportamiento y contratos.

No deben congelar apariencia o implementación interna.

## Automatizar

Cuando corresponda:

- routing;
- autorización;
- callbacks funcionales;
- state transitions;
- session lifecycle;
- PWA/runtime behavior;
- wake/activity behavior;
- data extraction/binding;
- configuration-driven rendering;
- empty/loading/error state contracts;
- alarm/KPI integration;
- asset availability cuando un JS/CSS sea requisito de composición;
- errores y fallbacks reales.

## No automatizar como contrato unitario

No crear asserts cuyo objetivo principal sea comprobar:

- colores;
- margin;
- padding;
- tamaños;
- selectores CSS;
- clases CSS;
- coordenadas/geometría;
- spacing visual;
- presencia/ausencia de funciones internas;
- nombres privados;
- estructura accidental de módulos.

## Qualification visual/manual

Corresponde validar visualmente:

- responsive;
- overflow;
- branding;
- espaciado;
- alineación;
- densidad;
- header;
- sidebar;
- modal shells;
- composición desktop/mobile/videowall.

Un finding visual puede originar un test automatizado sólo si se traduce a un comportamiento contractual estable.

## CSS histórico

Checkpoints antiguos mencionan gates/validadores CSS.

La búsqueda en el commit actual no encuentra `validate_css_tokens`.

Aunque un validador histórico exista en una versión anterior, la política vigente del Project lo supersede como criterio general de testing.

No reintroducir tests de CSS visual para mantener compatibilidad con ese historial.

## Principio

`test count != confidence`

La suite debe fallar cuando cambia un comportamiento importante, no cuando se reorganiza código o se calibra presentación.
