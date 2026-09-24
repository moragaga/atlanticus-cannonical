# Manager — Source Ledger

Estado: **AUDIT LEDGER / CURRENT CHECKPOINT ADDED**

## Autoridad

- `moragaga/atlanticus:main`: realidad implementada.
- `moragaga/atlanticus-cannonical:main`: autoridad documental vigente después de aplicar/revisar este paquete.
- `moragaga/atlanticus-decisions`: decisiones históricas y reglas congeladas explícitamente vigentes; no tratarlas automáticamente como implementación actual.
- Git: **SOLO LECTURA** para el asistente, salvo autorización explícita posterior.

## Corte histórico UI Profiles (conservado)

```text
moragaga/atlanticus@df5b99502265758e873e0565abf2176cc617104b
Parent: 31723a108ddd2f49346fdcbb844db9891eb08f4b
Tree: de1151ba72d44bc8ac6b6f2cfd6f57eb7e80c0a0
```

En ese corte:

```text
PROFILES-MANAGER-UI-REVIEW
CLOSED / VERIFIED MANUAL / CURRENT

ACCESS-UNRESTRICTED-PROFILES-CONTRACT
CLOSED / VERIFIED / CURRENT

ACCESS-MANAGER-UI-REVIEW
CLOSED / VERIFIED MANUAL / CURRENT
```

La evidencia UI de Profiles incluye:

- Source/Projection visibles por metadata inyectada; defaults genéricos `Profiles Source` / `Profiles Projection`.
- Etiquetas provider: local `Local Source` / `Local Projection`, Azure `Blob Storage` / `Cosmos DB`.
- ADA local: `Perfiles`, `Local Source`, `In-process Projection`.
- Paginación generic 10/20, empty state reservado, modal capability-local centrado en viewport.
- Avatar de perfiles normales con una inicial mayúscula; excepción visual `local` con inicial del primer y último nombre.
- Jane Doe y John Doe representables como `JD`, diferenciados por color; copy para perfiles de sistema y footer sin espacio artificial.

El usuario aceptó manualmente aquella UI. No se alteraron los contratos `ProfileDefinition`, `ProfileCatalog` ni `ProfilesConfiguration`. `compose_profiles_manager(...)` acepta `description`, `source_name` y `projection_name` sin cambiar defaults genéricos.

La qualification automatizada posterior a **aquel** checkpoint no quedó evidenciada en ese hito. No confundir la evidencia UI histórica con los tests actuales de ADA Generic ni adjudicar al monorepo resultados de un único paquete.

## Checkpoint de este cierre: ADA Generic Manager

```text
moragaga/atlanticus@ce1213ec14cdee0be905c042c1cf513d71fb5b2d
Parent observado: 842d9fa7a6ac8b96d0e021b8f349e0a56c26f055
Canonical previo inspeccionado: 6bd7f1f2616f954b422f3ddc1549a53a9b479682
Decisions consultadas: 50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

Código inspeccionado de ADA Generic:

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

**VERIFIED / CURRENT local:** 1F integró Manager con identidad local/Users; 1G construyó adaptadores reales; 1G.2 refinó la topología para separar Navigation y compartir `users-support` entre Profiles/Access; 1H incorporó el modo opt-in `durable` y CLI de recursos; 1H.1 eliminó contenedores Cosmos de `.env` y los resolvió por contratos internos. La versión intermedia 1G.1 y la configuración de contenedores Cosmos en `.env` están **SUPERSEDED**.

Evidencia de aceptación aportada por el usuario tras el último correctivo: **157 tests aprobados**, Ruff check/format correctos, mirrors validados y wheel construido. Esto verifica el paquete ADA Generic en el entorno local del usuario; no verifica infraestructura Docker/Azure, CI ni todo el repositorio.

**CURRENT del plan parcial:** un Cosmos endpoint/base para ADA Manager, `navigation-projection` independiente, `users-support` para Profiles/Access, `users-runtime` para Users promovidos y contenedores propios para Tool/KPI Registry/KPI Definition. Un Blob físico configurable aloja Source global, Source por Tool y Users Registry mediante prefijos lógicos. `ensure-local` sólo asegura la base/contendedores Cosmos tras validar Blob existente; `validate` no crea recursos.

**UNVERIFIED:** Entra productivo; integración externa Blob/Cosmos real, publicación/proyección, recovery/restart, creación de Blob, readiness global, pruebas transversales.

**VERIFIED STATIC / IMPACT UNVERIFIED:** la composición actual crea una instancia `AccessRuntime` en `bootstrap` para el principal Manager y otra dentro de `create_identity_module`. Falta una prueba real de coherencia de snapshots en el próximo foco; no concluir automáticamente que existe una vulnerabilidad ni declarar coherencia sin evidencia.

## Conflicto histórico independiente

```text
NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / VERIFIED CONFLICT / SEPARATE
```

La política declara `can_view(...)`; un consumidor histórico registró `can_access(...)`. Este hito no abordó su resolución y no autoriza alias de compatibilidad.

## Próxima frontera única

```text
ADA-GENERIC-DOCKER-REAL-PERSISTENCE-QUALIFICATION
PLANNED / NEXT
```

No abrir los frentes de alarmas ni Command Center ni modificar reglas globales del Manager durante el cierre del checkpoint.
