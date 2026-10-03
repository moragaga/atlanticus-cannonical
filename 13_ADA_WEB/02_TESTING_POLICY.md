# ADA Web — Testing Policy

Estado: **CURRENT — COMMENTED MIRROR TEST RETIREMENT CLOSED**

## Objetivo

Los tests Web protegen comportamiento y contratos funcionales, no la forma accidental de implementación.

## Automatizar

- routing y authorization behavior;
- callbacks funcionales;
- state transitions;
- session lifecycle;
- configuration-driven rendering;
- data binding/extraction;
- empty/loading/error behavior;
- recovery;
- persistencia;
- integración Alarm/KPI/Manager;
- asset availability sólo cuando su carga sea requisito funcional explícito.

## No automatizar como contrato

No crear ni mantener asserts cuyo objetivo principal sea comprobar:

- package version / `__version__`;
- `requires-python`;
- dependencias exactas o su orden;
- imports y arquitectura de imports;
- exports / `__all__`;
- existencia o ausencia de funciones/clases;
- estructura física del package;
- mirrors comentados;
- equivalencia AST, tokens o estructura entre productivo y `commented`;
- contenido exacto de distribución;
- colores, CSS, margin, padding, tamaños, selectores, clases, coordenadas o spacing visual.

Los tests estructurales existentes de estas categorías deben eliminarse en vez de actualizar sus expectativas durante una migración legítima.

## Commented mirrors CURRENT

Los mirrors bajo `commented/` continúan como material pedagógico.

No son una segunda implementación contractual y no requieren una suite que pruebe igualdad con productivo.

El retiro transversal local ejecutado en el hito Tool Contract Web Cutover eliminó funciones de test cuyo único propósito era comprobar igualdad de mirror y terminó con:

```text
No commented-mirror tests remain: PASS
```

La auditoría fue estructural respecto de la política de mirrors. No equivale a una ejecución del full monorepo.

## Packaging

`uv build`/instalación y smoke funcional prueban packaging. No congelar el wheel mediante inventarios de archivos/versiones salvo que exista un comportamiento de carga real que deba probarse.

## Dependencias de test

Las dependencias exclusivas de integración pertenecen a `dev`.

Ejemplo CURRENT: Alarm Delivery necesita Runtime y pandas para su prueba Engine→Delivery, pero no por eso los convierte en dependencias productivas.

Para source-path wiring de tests entre packages, preferir `conftest.py`; no usar pytest config como sustituto de una dependencia productiva real.

## Qualification visual/manual

Validar visualmente responsive, overflow, branding, espaciado, alineación, densidad y shell.

## Principio

```text
test count != confidence
```

La suite debe fallar cuando cambia comportamiento importante, no cuando se reorganiza código, se renombra una clase interna, se calibra packaging o se actualiza un mirror pedagógico.
