# Web Platform — Current Gaps

Estado: **VERIFIED / CANDIDATE**

## 1. Users / Navigation

### Bien separado

Packages core/configuration son independientes.

### Gap

ADA Configuration Manager enlaza Navigation con Users directamente para construir profile options.

Acción futura:
extraer ese enlace hacia una composition/binding opcional.

## 2. User Activity

### Existe

- event model;
- route changes;
- active time;
- route aggregates;
- Cosmos adapter;
- Memory adapter;
- Identity binding.

### Gap

No hay page visit history ordenada.

## 3. TTL

El contrato canónico requiere 24 h.

Debe verificarse dónde se declara físicamente el `CosmosContainerSpec` de User Activity porque no apareció en el barrido actual.

No asumir que el TTL está aplicado sólo porque el dominio lo requiere.

## 4. Cosmos provisioning

Existe y ya soporta:

- create database;
- ensure containers;
- validate containers;
- partition key validation;
- TTL validation.

Falta integrarlo como lifecycle de aplicación Web.

## 5. Storage provisioning

No se encontró equivalente a `CosmosProvisioner` en `connectivity/storage`.

Debe diseñarse únicamente si Source/Blob/bootstrap lo requiere.

## 6. Manager bypass

Actualmente `is_local` obtiene acceso completo en:

- Manager authorization;
- ADA Manager module helpers.

Debe retirarse.

## 7. Pre-Manager page

No existe todavía una superficie de bootstrap equivalente en la aplicación ADA Configuration Manager auditada.

## 8. Projection planner

Manager tiene workflows de proyección por módulo, pero aún debe congelarse un orquestador de proyecciones múltiples basado en dependencias.

## 9. Command Center

Debe aplicar el mismo modelo de:

- capability independence;
- resource bootstrap;
- readiness;
- projection orchestration;
- optional User Activity.
