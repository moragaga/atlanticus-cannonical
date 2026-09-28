# Users — Approved Registry, Consistency and Special Recovery

Estado: **DESIGN HANDOFF / NEXT INCREMENT / NOT IMPLEMENTED**

Inspección estática: `moragaga/atlanticus@ebc7a8bf8d49e931fd4e2487dac5ee036011a0a5`; decisiones de Manager consultadas en `moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`; base documental leída `moragaga/atlanticus-cannonical@7d0de8d9fa27f28170171588bc219c2bae99f34e`. Los acuerdos de producto de este hito aún no son código ni decisions formales.

## Objetivo — único siguiente foco

`USERS-PROJECTION-RECOVERY-001`: permitir distinguir usuarios aprobados de candidatos, validar el registro autorizado de Storage frente a usuarios promovidos en Cosmos y **reconstruir de forma explícita e invasiva** la proyección cuando un ambiente está vacío, desactualizado o se detectan cambios directos no autorizados en Cosmos. Esta capability será consumida por una futura página independiente de proyección; **NO** implementar la página ni cargos/área/grupo en este primer incremento.

## Evidencia CURRENT

```text
web/capabilities/users/core/src/atlanticus/web/users/models.py
  UserRecord(user_id, issuer, subject_id, profile_key, enabled, ...)
  UsersRegistrySnapshot(users, version)

web/capabilities/users/core/src/atlanticus/web/users/administration.py
  discover() -> candidatos/registrados/promovidos/conflictos
  promote() -> UsersRegistryStore.replace -> UsersAdministrationStore.create
  update()  -> UsersRegistryStore.replace -> UsersAdministrationStore.replace

web/capabilities/users/blob/src/atlanticus/web/users/blob/store.py
  <application_namespace>/users/users.json.gz
  registry version = Blob ETag

web/capabilities/users/cosmos/src/atlanticus/web/users/cosmos/store.py
  promoted users, identity-bound get/resolve; replace uses ETag

scopes/ada/web/application/ada-generic-application/src/ada/web/application/generic/manager_persistence.py
  users_registry = BlobUsersRegistryStore
  users_promoted = CosmosUsersStore (users-runtime)
  users-support = Profiles/Access projections, NO promovidos
```

La escritura actual entre Blob y Cosmos no es atómica. `discover` detecta diferencias pero no es un reconstructor de seguridad. El registry puede contener usuarios no promovidos; **la inclusión en `users.json.gz` no constituye por sí sola aprobación**.

## Invariantes acordadas (no inventar schema aún)

1. Atlanticus Users sigue genérico; conservar `user_id/issuer/subject_id`, `profile_key` y `enabled` como contratos de identidad/perfil. No introducir datos operacionales ADA ni un `extra` nuevo en esta etapa.
2. Source durable de usuarios autorizados vive en Storage; la proyección consumible vive en Cosmos. No ascender un registro alterado en Cosmos a autoridad.
3. Solo reconstruir el conjunto **explícitamente aprobado**. Nunca promover automáticamente Guest, identidades sólo descubiertas ni usuarios ajenos detectados en Cosmos.
4. No tomar un ETag entre ambientes como versión lógica portable: representa un objeto físico concreto. Si se necesita comparación portable, diseñar identidad/revisión desde contenido autorizado y contratos existentes, sin inventar un algoritmo obligatorio aquí.
5. La recuperación exige `validate` sin mutación, vista de discrepancias, confirmación explícita, ejecución auditada y política explícita para registros inesperados. Antes de borrar, revisar capacidades reales de `CosmosUsersStore` y pruebas de fallo; el store CURRENT no expone una operación pública de reconciliación/borrado integral.
6. Validar referencias a Profiles/Access y alcance de la configuración antes de reconstruir. No asumir que copiar usuarios entre directorios Entra distintos conserva `issuer + subject_id`.
7. Los reintentos no deben convertir modificaciones parciales en aprobaciones accidentales. Definir comportamiento ante Source que cambia durante la validación o ejecución, y ante fallos parciales de Cosmos.
8. No confundir recuperación de Cosmos con revocación inmediata de sesiones privilegiadas. Esa frontera debe auditarse y resolverse antes de exponer el proceso productivamente; cambios ordinarios pueden adoptar nueva información al refrescar la página, según la decisión del usuario.

## Preguntas OPEN — investigar en el siguiente chat, no rellenar

- ¿Cuál es la representación mínima del conjunto **aprobado** dado el `UsersRegistrySnapshot` actual, sin reinterpretar como aprobados los candidatos en Storage?
- ¿Cuándo queda consolidada una promoción si la escritura en Blob triunfa y Cosmos falla? ¿Cómo reparar sin introducir autorizaciones indebidas?
- ¿Qué validaciones y operaciones necesitan los stores existentes para comparar y reconstruir Cosmos sin borrar usuarios legítimos, incluidos cambios externos y concurrencia?
- ¿Qué significado tendrán diferencia de versión física, diferencia de datos e inconsistencia de identidad? ¿Qué resultado produce cada uno?
- ¿Cómo se define el ámbito compartido por aplicación/herramienta? CURRENT: registry usa `application_namespace`; no crear registro por herramienta sin una decisión explícita.
- ¿Qué condición verificable revoca privilegios a sesiones anteriores a recuperación? No afirmar protección absoluta frente a acceso total a infraestructura.
- ¿Qué comparación es viable al migrar DEV/UAT/PRD con mismo o distinto directorio Entra?

## Secuencia de trabajo para el siguiente chat

1. Auditar el HEAD nuevo de implementación, decisions y canonical; localizar código/tests reales, sin cambiar Git.
2. Debatir y congelar la representación del conjunto autorizado; decidir explícitamente reconciliación de `promote/update` con fallo parcial, más lectura/versión.
3. Definir el contrato de `validate` y recuperación (diferencias, precondiciones, concurrencia, auditabilidad, tratamiento de desconocidos y revocación).
4. Solo tras acuerdo, implementar un incremento mínimo backend + tests de comportamiento + espejos pedagógicos españoles. No modificar frontend ni página aislada en este primer incremento.

## Fuera de alcance explícito

- Implementación de la página aislada y tooling de paquetes/credenciales.
- Nueva capability ADA de cargos y asignación área/grupo; posible `users-support` debe verificarse antes.
- `UserRecord.extra` o `extra` en Cosmos, y potencial integración futura con Access. Sólo una idea futura, NO un contrato aprobado ni necesario para recovery.
- Refactorizaciones generales, procesos KPI/Alarm, multi-cloud, remigración Python, backend unrelated y legacy adapters.

Estado del entregable en este cierre: **PLANNED / UNVERIFIED / NEXT**. No hay tests de recovery nuevos ni esquema nuevo aprobado.
