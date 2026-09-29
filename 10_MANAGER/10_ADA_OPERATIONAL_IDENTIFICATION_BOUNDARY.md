# ADA Manager — Operational Identification Boundary

Estado: **PRODUCT-LEVEL DESIGN AGREED / PLANNED / NO IMPLEMENTATION — CONTRACT REFINEMENT OPEN**

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

**OPEN / UNVERIFIED — terminología:** en la planificación reciente se mencionó *operational scope*. Este documento solo tiene definido el campo **Área** (Mina / Planta); no está verificado que ambos conceptos sean equivalentes. Antes de definir modelos o formularios debe resolverse si se trata del mismo atributo, de un atributo diferente o de una relación. Hasta entonces no añadir campos, valores ni reglas derivados de ese término.

Los atributos se relacionarán mediante identidad estable ya promovida; conservar el contexto necesario de emisor/directorio, no asociar por nombre o correo. El acceso a la página debe concederse por la configuración normal de perfiles/permisos de ADA, **sin fijar un nombre de perfil privilegiado**. La clave de autorización concreta se decidirá al implementar.

## Fuente y proyección

La configuración de cargos y asignaciones será propia de ADA. El Source durable deberá seguir las reglas vigentes de Storage. La ubicación Cosmos de consumo se evaluará: `users-support` es candidato físico, NO decisión de topología definitiva. Antes de reutilizarlo verificar tipos documentales, particiones y ownership; no insertar atributos en `CosmosUsersStore` existente por comodidad.

La futura página aislada de proyección deberá incorporar estos Sources **después** de que el dominio exista y de que el proceso especial de usuarios haya resuelto las identidades correspondientes. No declarar dependencias exactas o schemas antes de auditar APIs.

## Continuidad propuesta — incrementos separados

1. **PROPOSED / DESIGN:** contrastar *operational scope* con Área; auditar contratos actuales de identidad promovida, catálogos, Source/Projection y autorización ADA. Definir primero identidad estable de asignaciones, reglas de integridad, permisos y contratos de persistencia, sin inventar topología Cosmos.
2. **PLANNED / BACKEND:** implementar el dominio propio de cargos y asignaciones, su Source durable y la proyección aprobada; probar contratos, referencias y persistencia.
3. **PLANNED / MANAGER:** integrar las pestañas Asignaciones y Cargos como módulo ADA gobernado por permisos del Manager, sin trasladar lógica de dominio a Atlanticus Manager genérico.
4. **PLANNED / QUALIFICATION:** probar el flujo desde usuarios promovidos reales hasta Source, proyección y lectura de asignaciones. Cualquier incorporación a Master exige otro diseño e incremento; no alterar sus seis pares actuales como efecto lateral.

## Ideas diferidas explícitamente

`extra` **sólo en Cosmos** como potencial extensión de una proyección consolidada de usuarios, con referencias a información operativa, y posible ampliación futura del ámbito de Access sobre asignaciones. Son ideas para analizar **después**; NO crear ahora campo, contenedor, readers, migración ni contrato de consumidores. El acceso CURRENT sigue determinado por `profile_key -> access_keys`.

## Estado / frontera

`ADA-OPERATIONAL-IDENTIFICATION`: **PLANNED / SEPARATE INCREMENT**. El diseño de producto de Asignaciones/Cargos está documentado, pero los contratos técnicos, la semántica de *operational scope*, la autorización concreta y la topología de consumo siguen **OPEN**. No se implementa código, esquema, UI ni integración Master en este cierre documental. Tras la reconciliación documental de Master, la calificación Docker de su distribución y el diseño de este dominio son frentes independientes; cada chat debe abrir uno solo.
