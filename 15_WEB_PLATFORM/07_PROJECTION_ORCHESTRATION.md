# Web Platform — Projection Orchestration

Estado: **CURRENT — MASTER 001A/001B/001C/001D.1/001D.2 IMPLEMENTED; 001D.3 LOCAL ACCEPTANCE CLOSED; USERS MASTER REPLACE BLOCKED**.  
Corte funcional de este documento: `moragaga/atlanticus@9c6daffd04b9c249f75a55b6cdb9b44e6d92a795` (2026-09-28). Al revisar `main`, el commit posterior `80748a21735a91d04f520ed4a8b9abf9dc9421da` afecta exclusivamente `kpi-runtime` y no estos contratos de Master. Canonical de partida: `atlanticus-cannonical@0f2fff3ec0e71903b5703e03dd6050765d9722ff`; decisions inspeccionadas parcialmente: `atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`. Las pruebas de ejecución se indican siempre como *VERIFIED USER-REPORTED*, no como CI ejecutada por el redactor.

## Contrato ordinario CURRENT — targets exactos

La identidad de una proyección es su `ProjectionTarget` completo, incluidas las dependencias normalizadas; un string privado de revisión no lo reemplaza. Las dependencias semánticas son del target y **no** implican un orden global entre dominios independientes. Reintentar un target ya aplicado no debe forzar una nueva proyección.

La composición Master reutiliza los seis pares de `ConfigurationManagerStores` y los servicios de proyección existentes:

| Dominio | Source → Projection | Prerrequisito Master |
|---|---|---|
| Navigation | `navigation` | Ninguno de los otros cinco |
| Profiles | `profiles` | Ninguno de los otros cinco |
| Tools | `tools` | Ninguno de los otros cinco |
| ADA Access | `ada-access` | `profiles` CURRENT |
| KPI Registry | `kpis` | `tools` CURRENT |
| KPI Definitions | `kpi-definitions` | `kpis` CURRENT |

Los estados ordinarios del planner son `SOURCE_MISSING`, `CURRENT`, `NEVER_PROJECTED`, `OUTDATED`, `BLOCKED`, `UNAVAILABLE`. Cada entrada expone Source release, target actual/ya proyectado, prerrequisitos, bloqueos y error tipo cuando corresponde. `MasterProjectionPlan.to_dict()` conserva **`mode: READ_ONLY`** porque el plan es una inspección; `ready_source_keys` **no** es un mandato durable ni una concesión de ejecución.

Código:

```text
scopes/ada/web/application/ada-generic-application/src/ada/web/application/generic/master_projection/
  plan.py
  composition.py
  apply.py
  web.py
```

## Master 001D.1 — executor CURRENT/CLOSED en su alcance

`MasterProjectionExecutor.apply(source_key, expected_target)` procesa **un dominio seleccionado**, no todos los `ready_source_keys`. Reinspecciona el plan actual; solo acepta estados `CURRENT`, `NEVER_PROJECTED` y `OUTDATED`, con `ProjectionTarget` esperado idéntico. Antes de escribir consulta nuevamente el target del servicio y la proyección activa. Si ya está alineado, responde `ALREADY_CURRENT`; si proyecta, verifica tanto el resultado recibido como la lectura persistida y el target corriente antes de responder `APPLIED`.

Los errores reportan razones tipadas (`INVALID_SELECTION`, `STALE_SELECTION`, estados bloqueados/ausentes, `UNAVAILABLE`, `EXECUTION_FAILED`, `VERIFICATION_FAILED`). No se promete transacción distribuida, rollback ni ejecución masiva. El executor no es por sí solo una frontera de autenticación HTTP.

## Master 001D.2 — HTTP prepare/confirm/apply CURRENT/CLOSED en código

La página independiente `GET/POST /master-projection` permite inspeccionar y, con material válido y **acción `projection.apply` autorizada**, preparar una selección `NEVER_PROJECTED`/`OUTDATED` y confirmarla explícitamente. La selección pendiente reside en la sesión Flask firmada, contiene `source_key`, representación del **target exacto** y `nonce`; confirmar consume ese pendiente. Se exige CSRF, sesión Master vigente, fingerprint del material, ámbito de aplicación/ambiente y permiso por operación. Justo antes de invocar el executor se reinspecciona el plan y compara exactamente el target pendiente; el executor vuelve a validar frente a los Stores/servicios actuales. Cambios de Source, estado o target obligan a preparar de nuevo. El botón no constituye autorización.

Sesión Master: 900 segundos desde emisión, sin prolongación automática por navegación; al reemplazar o retirar el material se invalida el fingerprint. Logout **POST** `/master-projection/logout` con CSRF. Solo estas **dos rutas exactas** se interceptan de manera independiente antes de Identity/Navigation: no crear un bypass de prefijo ni conceder permisos del Manager. Las respuestas Master aplican `Cache-Control: no-store, private` y cabeceras defensivas. La identidad de servicio no se convierte en `ManagerPrincipal`.

El formato de material declara `projection.preview`, `projection.apply`, `users.replace`; la última acción **no** implica controlador ni autorización de escritura sobre Users. Master no crea, edita ni publica Sources: eso continúa en Manager. Tampoco hay nuevo proceso remoto de coordinación.

## Users — excepción administrativa CURRENT y separada

`UsersAdministrationService` mantiene discover/promote/update. `UsersApprovedRecoveryService` conserva capture/validate/restore/validate_replace/replace_approved desde **snapshots expresamente aprobados**. El Registry Blob puede contener candidatos y no equivale a un snapshot autorizado. La pantalla **Proyección de usuarios** del Manager utiliza `users.manage` y no se confunde con Master.

Master consulta el catálogo de snapshots como estado separado: `CATALOG_UNAVAILABLE`, `SNAPSHOT_MISSING`, `PROFILES_PENDING`, `SNAPSHOT_SELECTION_REQUIRED`; expone `operation: users.replace` con **`executable: false`**. `users.replace` desde Master permanece **BLOCKED**, fuera de los seis dominios y del cierre 001D.

## Qualification acotada

- **VERIFIED STATIC:** módulos y pruebas `test_master_projection_*.py` publicados en `atlanticus@9c6daffd`. La implementación 001D.1/001D.2 está en `ca3ee5084542e393c105b49e98b3c282da56f7fb` y permanece en el corte 9c6.
- **VERIFIED USER-REPORTED TESTS:** selección 001D.2 de **50/50** pruebas, Ruff y `git diff --check` PASS antes del commit correspondiente; no equivale a CI de todo el repositorio.
- **VERIFIED USER-REPORTED LOCAL DOCKER — 001D.3:** distribución del HEAD `ca3ee508`, 67 wheels, `BUILT_UNQUALIFIED`, `PRECHECK_PASS`, `SYNCED`; material externo real generado y acceso Master; emuladores Docker accesibles; seis filas inicialmente observadas `SOURCE_MISSING`; el operador modificó/publicó Navigation, ejecutó prepare/confirm en Master y, al recargar Manager, observó **proyectado y sincronizado**. Este es el recorrido UI positivo de **Navigation solamente**, no una comprobación manual de los otros cinco dominios ni un trace independiente de cada escritura Cosmos.
- **UNVERIFIED:** Docker de los otros cinco dominios, carreras/fallos parciales reales, auditoría operativa completa, multiworker/CI global y Azure/Entra productivos.

## Decisiones, refinamientos y fronteras

- **SUPERSEDED:** “001D Apply solo planificado/no implementado” de versiones anteriores de este documento. El código backend y HTTP está implementado y probado en su alcance; no extender esta afirmación a Users REPLACE.
- **CURRENT:** Source durable en Blob para dominios migrados, Cosmos como proyección de consumo, targets exactos, revalidación antes de escritura, confirmación humana, permisos por operación y no reproyección del target vigente.
- **OPEN / UNVERIFIED CONTRACT:** la excepción Master de credencial propia respecto de la baseline histórica Entra pre-Manager precisa contraste exhaustivo de DOCX de decisiones y resolución formal **antes de producción**. Las reglas globales Markdown del Manager no resuelven por sí solas ese punto.
- **OPEN / OPERATIONS:** material en host productivo, custodia/rotación/revocación operacional; `ADA_MASTER_PROJECTION_MATERIAL_PATH` es una ruta externa, no un uploader/warmup automático.
- **BLOCKED / OTHER FOCUS:** Users Master REPLACE hacia destinos sin promovidos (`identity_realm`), mantenimiento/revocación e interrupción/reintento real.

Próximo foco único de este cierre: **consolidar la documentación canónica de Master y distribución ADA**. No abrir código, refactor, Users, alarmas, KPI, migración Python ni infraestructura productiva en este traspaso.
