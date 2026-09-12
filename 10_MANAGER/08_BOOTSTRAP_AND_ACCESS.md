# Manager — Bootstrap and Access

Estado: **CURRENT DIRECTION**

## Problema actual

Manager utiliza perfiles y access keys para proteger módulos.

Sin embargo existe bypass implícito:

```text
principal.is_local
→ full access
```

y la composición ADA repite la misma excepción para Users, Navigation, Tools y KPI.

Esto debe retirarse.

## Objetivo

Separar:

```text
BOOTSTRAP ACCESS
        ≠
MANAGER ACCESS
```

### Bootstrap

Puede existir antes de:

- Users projection;
- Profiles efectivos;
- Navigation projection.

### Manager

Sólo queda habilitado después de que sus dependencias mínimas estén listas.

Utiliza autorización normal.

## Superficie previa

Una página independiente del Manager debe permitir:

- revisar infraestructura;
- visualizar Source releases/files;
- calcular plan de proyección;
- proyectar configuración;
- ver bloqueos/errores;
- determinar Manager readiness.

La ruta/nombre exactos quedan abiertos.

## Producción

La página puede estar antes de Users/Profile app config, pero no debe ser anónima para acciones privilegiadas.

Utiliza Entra/bootstrap authorization independiente.

## Local

Provider local puede habilitar el flujo de desarrollo, pero no implica `administrator`.

## Navigation / Users

Manager debe poder instalar:

```text
Users only
Navigation only
Users + Navigation
```

sin que Navigation requiera Users.

Cuando Navigation usa perfiles:

```text
optional Users↔Navigation binding
```

es quien aporta profile options.
