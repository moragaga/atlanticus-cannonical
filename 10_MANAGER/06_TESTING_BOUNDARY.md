# Manager — Testing Boundary

Estado: **CURRENT POLICY**

## Automatizar

Probar comportamiento real:

- autorización;
- registry/routing;
- lifecycle;
- draft persistence semantics;
- validation;
- source verification;
- conflict behavior;
- publish/project transitions;
- history semantics;
- callbacks críticos;
- errores;
- integridad referencial;
- session reuse;
- backend concurrency contract.

## No congelar con tests

No añadir tests cuya finalidad sea:

- CSS visual;
- margin/padding/color/tamaño;
- clases visuales concretas;
- detalles de layout sin comportamiento contractual;
- existencia/no existencia de funciones internas;
- nombres privados;
- estructura accidental.

Los assets JS/CSS pueden verificarse como presentes/cargables cuando eso sea requisito de composición.

## Qualification visual

Se valida fuera del unit test:

- responsive;
- overflow;
- spacing;
- branding;
- header;
- modal shell;
- densidad;
- apariencia.

Un finding visual real puede justificar una prueba funcional si se identifica un contrato automatizable, pero no debe convertirse en assert de CSS arbitrario.
