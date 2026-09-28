# Manager — Bootstrap and Access

Estado: **CURRENT / ACCESO ADMINISTRATIVO VIGENTE / USERS RECOVERY PLANNED / DESPLIEGUE EXTERNO PLANNED**

Corte de inspección estática: `moragaga/atlanticus@ebc7a8bf8d49e931fd4e2487dac5ee036011a0a5` (2026-09-27). Este documento conserva las fronteras CURRENT e incorpora únicamente las decisiones y pendientes del cierre; no afirma implementación de los flujos nuevos.

## Fronteras CURRENT y congeladas

`BOOTSTRAP ACCESS != MANAGER ACCESS`. Identity/Users rechaza identidad inválida y usuario promovido deshabilitado; una identidad autenticada sin `UserRecord` puede producir `READY`, **sin otorgar por ello permisos Manager**. El principal administrativo expone `access_keys`; `ManagerAuthorizationPolicy.can_view(principal, item)` exige una clave concedida para visualizar un `ManagerModule` o `ManagerEntry`. Las acciones Source/Projection del Coordinator vuelven a comprobar acceso. `is_local` por sí solo no concede acceso.

Claves del Manager ADA actualmente compuestas: `users.manage`, `profiles.manage`, `access.manage`, `navigation.manage`, `tools.manage` y `kpis.manage`. Users sigue siendo `ManagerEntry`, sin Source/Projection sintéticos ni borrador global. Profiles es capability genérica; Access es dominio ADA y resuelve `profile_key -> access_keys`. Los perfiles irrestrictos `root`/`local` pertenecen al contrato de Access; no permiten introducir un bypass nuevo en Manager.

## Estado CURRENT de identidad

- En local, `LocalIdentityProvider` y `ATLANTICUS_LOCAL_IDENTITY_SUBJECT_ID` sirven para desarrollo. El CLI puede seleccionar Jane o John cuando no se configura subject. El contexto local confiable y los colores del usuario reconocido son decisiones visuales previas; no equivalen a identidad productiva.
- En producción, el Starter de ADA aún exige que el host suministre su `IdentityProvider`: `application/production.py` es un punto de integración que falla si no se implementa. No declarar Entra productiva como desplegada o calificada por haber implementado la interfaz de identidad.
- El middleware de Identity resuelve de nuevo identidad/Users en solicitudes de documento HTML; otras solicitudes pueden reutilizar snapshots vigentes de sesión. Las sesiones Flask se firman; el cliente y `localStorage` no son autoridad. **UNVERIFIED:** calificación integral de revocación inmediata en todos los endpoints/callbacks administrativos.

## Users CURRENT: registro y proyección, sin workflow sintético

Persistencia observada en la composición durable:

```text
Blob UsersRegistryStore
  <application_namespace>/users/users.json.gz
       -> promote/update de UsersAdministrationService
Cosmos UsersRuntimeStore / UsersAdministrationStore
  usuarios promovidos para resolución runtime
Cosmos users-support
  proyecciones Profiles y ADA Access; NO equivale a Users Runtime
```

En `promote` y `update` se escribe el registro en Blob antes de crear/reemplazar el promovido de Cosmos. Se utilizan versiones del registro y ETags para escrituras; **no hay atomicidad entre los dos almacenes**. `discover()` compara candidatos, registrados y promovidos, y exhibe discrepancias; no equivale a validación/reconciliación integral de seguridad. El registro en Blob puede incluir candidatos no promovidos: no tratar su presencia como aprobación suficiente.

## USERS-PROJECTION-RECOVERY — diseño acordado / aún NO implementado

Se necesita una operación especial de validación y reconstrucción de la proyección de usuarios desde un conjunto **explícitamente aprobado** en Storage. No invertir la autoridad ni convertir cambios manuales en Cosmos en fuente legítima. Debe cubrir ausentes, cambios de `profile_key`/`enabled`, identidades discrepantes y presentes en Cosmos no aprobados. Antes de recuperar, validar que la fuente y sus referencias de Profiles/Access sean utilizables; exigir confirmación debido al carácter invasivo. Reintentos y consecuencias de fallos parciales se definirán sobre los stores reales; no prometer borrado ni limpieza con un método aún inexistente.

La lectura ordinaria puede conservar las reglas de refresco de página acordadas para cambios habituales. **OPEN de seguridad:** reevaluación de autorización en operaciones sensibles y tratamiento de sesiones anteriores a una recuperación. Restaurar Cosmos no revoca por sí solo sesiones ya emitidas.

Contrato y riesgos para el siguiente incremento: `15_WEB_PLATFORM/13_USERS_PROJECTION_RECOVERY.md`.

## Página aislada de proyección — decisión de producto / implementación PLANNED

La nueva página **no forma parte del Manager** ni concede navegación administrativa. Su función es operar proyecciones disponibles desde los Source ya preparados en Storage de un ambiente, incluyendo la nueva reconciliación especial de Users. Puede inicializar UAT/PRD sin necesitar usuarios promovidos previamente ni editar módulos uno a uno.

El equipo del proyecto generará por tooling el material protegido de acceso/despliegue; la operación solicita usuario y contraseña de servicio. Requisitos expresos: credenciales sin vencimiento automático, paquete no marcado como consumido, reintento del mismo paquete cuando falle una ejecución y reemplazo manual del material si se pierden credenciales. No almacenar contraseñas recuperables dentro del paquete. El hash sirve para verificación, **no** para descifrado de contenido; el diseño criptográfico concreto, la protección del verificador y la autoridad de despliegue previa a Users permanecen OPEN. No dar por desarrollado ningún endpoint, esquema o ZIP nuevo.

Esta superficie no sustituye la identidad normal del Manager ni autoriza mutaciones anónimas.

## Reglas congeladas

1. Atlanticus Users, Manager e Identity siguen genéricos; ADA define sus propias configuraciones de dominio.
2. La autorización viene del servidor y de `profile_key -> Access`; ni cargo, ni grupo, ni área conceden permisos.
3. No incluir Guest como usuario administrado o promovido en recuperaciones. `guest` puede existir como representación transitoria conforme al contrato vigente, no como promoción final.
4. No confiar en un perfil enviado desde JavaScript o `localStorage`; no usar usuarios de Cosmos como autoridad para corregir Blob.
5. No crear Source/Projection sintéticos para `ManagerEntry` de Users ni un bypass del Manager para desplegar.
6. No acoplar el despliegue aislado a perfiles todavía ausentes; su autorización independiente debe definirse y verificarse antes de exponerlo.

## Estado

```text
IDENTITY + MANAGER AUTH CURRENT                         CURRENT
USERS BLOB REGISTRY + COSMOS PROMOTED                  CURRENT / VERIFIED STATIC
USERS SPECIAL VALIDATE / REPROJECT                   PLANNED / UNIMPLEMENTED
PRE-USERS ISOLATED DEPLOYMENT PAGE                    PLANNED / UNIMPLEMENTED
ENTRA PRODUCTIVE HOST                                 PLANNED / UNVERIFIED
PRIVILEGED ACTION REVOCATION / SESSION TREATMENT      OPEN / UNVERIFIED
```

La arquitectura y el próximo foco se congelan aquí; ninguna operación nueva se implementa en este cierre documental.
