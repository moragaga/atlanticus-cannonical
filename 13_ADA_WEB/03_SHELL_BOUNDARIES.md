# ADA Web — Shell Boundaries

Estado: **CURRENT**

## ADA Operational Shell

`ada-generic-application` compone el shell operacional y utiliza el ADA operational header.

Responsabilidad:

- identidad operacional;
- navegación ADA;
- estado/contexto operacional;
- superficies KPI/Alarm;
- interacción de la Tool.

## Manager Shell

`ada-configuration-manager` monta una superficie Manager separada sobre `atlanticus.web.manager`.

Manager posee:

- Home `/manager`;
- header administrativo propio;
- sidebar/navegación administrativa;
- module registry;
- workflow administrativo.

## Regla

No fusionar:

- header Manager;
- header ADA;
- navegación Manager;
- navegación operacional ADA.

Pueden compartir:

- primitives;
- design tokens;
- branding;
- componentes genéricos cuando corresponda.

Pero el ownership permanece separado.

Esto no es una regla estética: evita acoplar administración y operación en una sola aplicación conceptual.

## System / Bootstrap Surface

Existe una tercera frontera de shell mínima para startup/readiness.

No reemplaza:

- ADA Operational Shell;
- Manager Shell.

Su finalidad es que la aplicación pueda explicar y preparar su estado antes de que las capas funcionales estén listas.

No debe convertirse en un nuevo dashboard operacional.
