# ADA Manager — Operational Identification Boundary

Estado: **DESIGN AGREED AT PRODUCT LEVEL / PLANNED / NO IMPLEMENTATION**

## Propósito

Identificar operacionalmente a usuarios ADA existentes para consumo futuro de gestión de alarmas, **sin modificar Atlanticus Users** ni utilizar área/cargo/grupo como sustitutos de Profiles o Access.

## Página futura

Un módulo propio de ADA bajo Manager con dos pestañas internas:

- **Asignaciones:** seleccionar exclusivamente usuarios ya promovidos/proyectados y asociar información opcional.
- **Cargos:** crear, mantener y desactivar manualmente el catálogo de cargos específicos del proyecto.

Campos de asignación:

| Campo | Valores previstos | Estado inicial |
|---|---|---|
| Área | Mina / Planta | No informado (`null`) |
| Cargo | Identidad de catálogo manual propio | No informado (`null`) |
| Grupo | 1, 2, 3, 4 | No informado (`null`) |

Los tres campos son opcionales indefinidamente; cargos externos a sala no deben recibir datos ficticios para completar formularios. Guest no necesita asignación operacional. No introducir datos operacionales en los documentos genéricos de Users ni cambiar sus pantallas existentes.

Los atributos se relacionarán mediante identidad estable ya promovida; conservar el contexto necesario de emisor/directorio, no asociar por nombre o correo. El acceso a la página debe concederse por la configuración normal de perfiles/permisos de ADA, **sin fijar un nombre de perfil privilegiado**. La clave de autorización concreta se decidirá al implementar.

## Fuente y proyección

La configuración de cargos y asignaciones será propia de ADA. El Source durable deberá seguir las reglas vigentes de Storage. La ubicación Cosmos de consumo se evaluará: `users-support` es candidato físico, NO decisión de topología definitiva. Antes de reutilizarlo verificar tipos documentales, particiones y ownership; no insertar atributos en `CosmosUsersStore` existente por comodidad.

La futura página aislada de proyección deberá incorporar estos Sources **después** de que el dominio exista y de que el proceso especial de usuarios haya resuelto las identidades correspondientes. No declarar dependencias exactas o schemas antes de auditar APIs.

## Ideas diferidas explícitamente

`extra` **sólo en Cosmos** como potencial extensión de una proyección consolidada de usuarios, con referencias a información operativa, y posible ampliación futura del ámbito de Access sobre asignaciones. Son ideas para analizar **después**; NO crear ahora campo, contenedor, readers, migración ni contrato de consumidores. El acceso CURRENT sigue determinado por `profile_key -> access_keys`.

## Estado / frontera

`ADA-OPERATIONAL-IDENTIFICATION`: **PLANNED**. No código, tablas, tests, UI ni esquema aprobados en este cierre. El único siguiente foco del proyecto sigue siendo `USERS-PROJECTION-RECOVERY-001`.
