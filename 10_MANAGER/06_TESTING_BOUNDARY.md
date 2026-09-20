# Manager — Testing Boundary

Estado: **CURRENT POLICY / REFINED FOR UI REVIEW**

## Principio

Los tests automatizados deben proteger comportamiento, contracts, invariantes, regresiones y
flujos críticos.

No deben congelar presentación accidental ni implementación interna.

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
- backend concurrency contract;
- paginación funcional: page calculation, límites, cambios de página y callbacks cuando sean
  contractualmente relevantes;
- carga de un asset JS/CSS sólo cuando su presencia/carga sea parte explícita del contract.

## No congelar con tests

No añadir ni conservar tests cuya finalidad sea validar:

- CSS visual;
- margin/padding/color/tamaño;
- estilos concretos;
- clases CSS visuales concretas;
- estructura de markup sin comportamiento contractual;
- responsive visual;
- overflow visual;
- forma visual de paginación;
- contenido o estructura interna de JavaScript;
- existencia/no existencia de funciones internas;
- existencia/no existencia de clases internas;
- nombres privados;
- estructura accidental del package;
- una implementación interna concreta cuando el comportamiento observable ya está cubierto.

Un test no se vuelve válido por comprobar que una clase, función o selector dejó de existir
después de un refactor.

## Regla para el próximo UI review

Si durante `MANAGER-UI-CONSISTENCY-REVIEW` aparece un test existente cuyo único propósito
viola la frontera anterior:

```text
REMOVE
```

No modificar la nueva UI para satisfacerlo.

No reescribir el test para congelar una nueva estructura visual equivalente.

Si detrás del test existe un comportamiento funcional real, reemplazarlo únicamente por una
prueba de ese comportamiento observable.

## Assets

La excepción permitida es estrecha:

```text
asset JS/CSS required by composition
→ puede probarse como presente/cargable
```

No probar:

```text
selectores internos
reglas CSS
nombres de clases visuales
contenido JS
funciones JS internas
estructura del archivo
```

## Qualification visual

Se valida visualmente:

- responsive;
- overflow;
- spacing;
- branding;
- header;
- modal shell;
- densidad;
- apariencia;
- alineación;
- consistencia entre superficies;
- presentación de paginación.

Un finding visual real puede justificar una prueba automatizada sólo si se identifica un
comportamiento funcional independiente de la apariencia.

## Secuencia actual

```text
MANAGER-UI-CONSISTENCY-REVIEW
PLANNED / NEXT

MANAGER-REAL-PERSISTENCE-QUALIFICATION
PLANNED / AFTER UI REVIEW
```

No usar el UI review para abrir qualification de persistencia ni otros frentes backend.
