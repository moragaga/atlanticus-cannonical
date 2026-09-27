# Manager — Source Ledger

Estado: **AUDIT LEDGER / LOCAL HEADER + IDENTITY CHECKPOINT ADDED 2026-09-27**

## Autoridad

- `moragaga/atlanticus:main`: realidad implementada. Inspección de este cierre: `392ee281a32396516fb08c23c63514d8cbdb3489`.
- `moragaga/atlanticus-cannonical:main`: autoridad documental vigente después de integrar/revisar este paquete; base leída: `a4c813bf6c833455ebe4f5f0a5968b0633c7b045`.
- `moragaga/atlanticus-decisions`: decisiones históricas y reglas congeladas explícitamente vigentes; HEAD consultado `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`.
- Git: **SOLO LECTURA** para el asistente, salvo autorización explícita posterior.

## Corte histórico UI Profiles (conservado)

```text
moragaga/atlanticus@df5b99502265758e873e0565abf2176cc617104b
Parent: 31723a108ddd2f49346fdcbb844db9891eb08f4b
Tree: de1151ba72d44bc8ac6b6f2cfd6f57eb7e80c0a0
```

En ese corte:

```text
PROFILES-MANAGER-UI-REVIEW                  CLOSED / VERIFIED MANUAL / CURRENT
ACCESS-UNRESTRICTED-PROFILES-CONTRACT       CLOSED / VERIFIED / CURRENT
ACCESS-MANAGER-UI-REVIEW                    CLOSED / VERIFIED MANUAL / CURRENT
```

La evidencia UI de Profiles incluye Source/Projection visibles por metadata inyectada y defaults genéricos `Profiles Source` / `Profiles Projection`. Proveedores locales `Local Source` / `Local Projection`, Azure `Blob Storage` / `Cosmos DB`. ADA local: `Perfiles`, `Local Source`, `In-process Projection`. Paginación generic 10/20, empty state reservado, modal capability-local centrado. Avatar de perfiles normales con inicial mayúscula; excepción histórica `local` con inicial del primer y último nombre. Jane/John representables como `JD`, diferenciados por color. Copy para perfiles de sistema y footer sin espacio artificial.

El usuario aceptó manualmente aquella UI. No se alteraron `ProfileDefinition`, `ProfileCatalog` ni `ProfilesConfiguration`. `compose_profiles_manager(...)` acepta `description`, `source_name` y `projection_name` sin cambiar defaults genéricos. La qualification automatizada posterior a **aquel** checkpoint no quedó evidenciada en ese hito. No atribuirle el resultado de un paquete a todo el monorepo.

## Checkpoint histórico ADA Generic Manager (conservado)

```text
moragaga/atlanticus@ce1213ec14cdee0be905c042c1cf513d71fb5b2d
Parent: 842d9fa7a6ac8b96d0e021b8f349e0a56c26f055
Canonical previo inspeccionado: 6bd7f1f2616f954b422f3ddc1549a53a9b479682
Decisions consultadas: 50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

Código inspeccionado entonces:

```text
scopes/ada/web/application/ada-generic-application/
  .env.detail
  src/ada/web/application/generic/settings.py
  src/ada/web/application/generic/bootstrap.py
  src/ada/web/application/generic/__main__.py
  src/ada/web/application/generic/manager_principal.py
  src/ada/web/application/generic/manager_persistence.py
  src/ada/web/application/generic/manager_deployment.py
  tests/test_manager_persistence.py
  tests/test_manager_deployment.py
```

**VERIFIED / CURRENT local en aquel corte:** 1F integró Manager con identidad local/Users; 1G construyó adaptadores reales; 1G.2 refinó topología: Navigation independiente, `users-support` compartido Profiles/Access; 1H incorporó modo opt-in `durable` y CLI de recursos; 1H.1 eliminó los nombres de contenedores Cosmos de `.env` y los resolvió por contratos internos. 1G.1 y contenedores físicos de Cosmos en `.env` quedaron **SUPERSEDED**.

Evidencia entonces aportada: **157 tests aprobados**, Ruff check/format, mirrors y wheel. No demuestra infraestructura Docker/Azure, CI ni todo el repositorio.

El plan parcial conserva un Cosmos endpoint/base para ADA Manager; `navigation-projection` independiente, `users-support` Profiles/Access, `users-runtime` promovidos y contenedores propios Tool/KPI. Blob físico configurable aloja Source global, Source por Tool y Users Registry mediante prefijos lógicos. `ensure-local` asegura Cosmos tras validar Blob existente; `validate` no crea recursos.

**UNVERIFIED entonces y aún fuera de este cierre:** Entra productiva, integración real Blob/Cosmos, publicación/proyección, recovery/restart, creación Blob, readiness global, pruebas transversales.

**Finding estático histórico pendiente de revalidación:** se observaron instancias de `AccessRuntime` creadas en `bootstrap` y `create_identity_module`; se pidió prueba real de coherencia de snapshots. No declarar fallo ni coherencia sin evidencia nueva.

**Finding histórico separado:** `NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT` registró consumer `can_access(...)` frente a `can_view(...)`. No asumir que persiste en HEAD sin inspección; no introducir alias.

## Nuevo checkpoint — header local y colores Jane/John

```text
Implementation inspected: moragaga/atlanticus@392ee281a32396516fb08c23c63514d8cbdb3489
Canonical before candidate: moragaga/atlanticus-cannonical@a4c813bf6c833455ebe4f5f0a5968b0633c7b045
Decisions consulted: moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

- Tras 01B branding, en 01C el usuario aportó `CHECK_PASS`, `APPLY_PASS`, **16 tests** Manager y **12 tests** ADA Configuration Manager, `git diff --check` PASS; quitó Los Pelambres del header y cambió el label de ADA a `Usuarios`. En una etapa intermedia se situó el nombre de usuario debajo de los enlaces.
- El usuario publicó `716b98973e013c5da8ffa9be75e6b0deb5af832e` tras 01C. 01D reemplazó esa decisión intermedia: se retiró por completo el **nombre visible del header**, conservando `ManagerPrincipal` y autorización. **VERIFIED MANUAL** por el usuario. La salida de tests posterior a 01D no fue proporcionada en este chat: **UNVERIFIED**.
- El usuario aportó `b600ca591b56d0924aed752dfae6e9fab2c6f1d6` como corte previo a 02. La primera variante 02 (avatar usuario con insignia Local azul fija) quedó **SUPERSEDED** por el correctivo: avatar **e insignia Local** usan la paleta del usuario reconocido, Jane rosa `#C85D91`, John azul `#3778C2`. Selección aleatoria si no se fija `ATLANTICUS_LOCAL_IDENTITY_SUBJECT_ID`.
- **VERIFIED MANUAL:** el usuario aceptó el color de Jane y John y la selección automática; **UNVERIFIED como ejecución aquí:** terminal de la suite final corregida.
- **VERIFIED STATIC en HEAD inspeccionado:** el header no muestra `principal.display_name`; ADA inyecta dos marks, product ADA + framework Atlanticus; `wiring.py` usa `title='Usuarios'`; `navigation_binding.py` resuelve ambos colores desde `LOCAL_USERS` por `subject_id` en contexto local; `test_local_navigation_avatar.py` contiene regresiones de ambos usuarios, alias, fallback, usuario administrado y selector automático. Existencia de tests no es resultado de ejecución.

## Límite de transferencia

La aceptación de **ADA Generic core local** no califica Manager desde un **Starter ADA distribuido** que históricamente fue probado con Manager `disabled`. Siguen abiertos recorrido HTML de `/example`, Blob/Cosmos real y restart, producto Entra y preparación de distribución/deployment. La siguiente frontera de este chat es auditoría de artifacts; no reabrir diseño del header.
