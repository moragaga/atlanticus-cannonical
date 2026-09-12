# Manager — Navigation and Home

Estado: **CURRENT**

## `/manager`

`/manager` es una Home real.

No representa el primer módulo y no redirige implícitamente a Users/Navigation/Tools.

## Registry

`ManagerModuleRegistry` es la fuente de módulos visibles y rutas administrativas.

Home y sidebar deben derivar del mismo registry para evitar divergencia.

## Home

Responsabilidad:
- descubrir configuraciones disponibles;
- mostrar estado resumido;
- abrir módulo.

No debe absorber workflow de edición/publicación.

## Navegación Manager

Manager conserva navegación administrativa propia:

- botón/trigger lateral;
- Home;
- módulos;
- retorno explícito a Manager Home;
- sidebar administrativa.

No fusionar este mecanismo con la navegación operacional principal de ADA.

## Paginación

La Home actual implementa page size 6.

La paginación de presentación no debe provocar lecturas repetidas de Source cuando el snapshot ya está hidratado.

## Principio

La navegación de Manager organiza capacidades administrativas.

La navegación ADA organiza herramientas/superficies operacionales.

Son responsabilidades distintas aunque convivan en el mismo ecosistema.
