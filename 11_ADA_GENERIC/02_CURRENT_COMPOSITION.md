# ADA Generic — Current Composition

Estado: **CURRENT / CORE STAGE 1 CLOSED / MANAGER AUTHORIZATION CONVERGED / GENERATED TOOL EXTENSION CLOSED / RUNTIME QUALIFICATION NEXT**

Corte inspeccionado:

```text
moragaga/atlanticus@a75465745e188da4765e803595b17acaa55d9306
```

Decisions inspeccionado:

```text
moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

Canonical base reemplazada por este documento:

```text
moragaga/atlanticus-cannonical@e5f22298b9a8d62182cf9dc5bcad46c971faf261
```

## Composition CURRENT

ADA Generic compone capabilities independientes y consume Tool Projection durable.

```text
AdaGenericSettings
→ ToolPersistenceComposition
→ resolve_operational_tool_projection()
→ READY | UNCONFIGURED | UNAVAILABLE | INVALID
→ ADA Generic Web
```

Runtime ordinario no necesita Tool Source cuando existe Projection válida.

Con Tool READY y KPI Delivery configurado puede adjuntar `AdaKpiCollector`.

`OperationalRenderBinding` conserva estructura; no transporta KPI state.

La composición operacional genérica incluye, según configuración, módulos compartidos de UI, alarm surfaces, branding, Navigation, shell/header y runtime experience. El layout operacional mantiene Navigation habilitada.

## Manager + Identity CURRENT

Manager se integra al disponer de stores/dependencies.

Autenticación, ADA Access, Navigation y Manager authorization son fronteras distintas.

Composición del principal Manager:

```text
managed root
→ administrative_override=True

trusted local + local environment
→ administrative_override=True

basic / guest / custom / unknown
→ no administrative override

bootstrap root
→ no implicit Manager administration
```

`ManagerPrincipal.access_keys` permanece vacío en estas rutas.

ADA Generic ya no lee `AdaAccessConfiguration` ni `ProfileCatalog` para derivar permisos Manager.

El Configuration Manager local usa override en lugar de una lista agregada de permisos.

## Manager authorization CURRENT

```text
manager_access_granted(principal, access_key)

None          → DENY
override      → ALLOW
granular key  → ALLOW
otherwise     → DENY
```

Los `can_manage` internos de ADA Configuration Manager usan la misma semántica.

Cuando `create_operational_application_runtime()` dispone de Manager dependencies/stores, integra `build_configuration_manager_surface(...)` bajo el route prefix del Manager y agrega su package de páginas sin convertir esas páginas en código copiado dentro de cada Tool.

## Generated Tool extension CURRENT

La aplicación generada ADA ya no hereda las páginas/módulos demo del starter Generic.

Su composición específica:

```text
create_local_operational_composition()
→ conserva módulos/layout ADA
→ agrega create_application_modules()
→ page_packages = ('application.pages',)
```

La Tool generada posee:

```text
src/application/pages/home.py
src/application/pages/__init__.py
src/application/modules/__init__.py
```

Contrato de ownership:

```text
ADA Generic
├── shell/header
├── Navigation
├── Manager integration
├── branding/runtime experience
├── persistence/bootstrap contracts
└── capacidades ADA reutilizables

Generated Tool
├── Home específico
├── pages adicionales
├── módulos/callbacks propios
├── assets/presentación específica
└── integración particular del host cuando corresponda
```

El Home específico registra `/` y vive en `application.pages`. No se mantiene un alias, shim o compatibilidad legacy con `application.modules.example` ni con el Home Generic anterior.

## Distribution/project tooling CURRENT

No confundir esta frontera con el dominio `ToolConfiguration`.

El tooling de generación/distribución del proyecto ADA está bajo:

```text
tooling/distribution/web/
```

El project tooling reusable fue extraído de la Tool generada a:

```text
tooling/distribution/web/ada/project-tooling/
```

La implementación reusable se distribuye como:

```text
ada-project-tooling==0.1.0
```

El `tooling/project.py` de la Tool es un bootstrap delgado que carga la implementación reusable y, en artifact distribuido, verifica el wheel por identidad y SHA256 antes de ejecutarlo.

Los comandos humanos Web/ADA exponen `.sh` y `.cmd`; el `.py` queda como implementación.

Esta frontera de **distribution/project tooling** está CLOSED para el alcance del hito `a7546574`. No reabrirla confundiendo el término “Tooling” con configuración de Tool.

## Tool domain contract CURRENT observado

`ToolConfiguration` contiene:

```text
tool_key
display_name
kind
source_consumption
source_operational_participation
structure
branding
```

`ToolSourceConsumption` representa:

```text
tool_key
source_keys
```

Kinds observados:

```text
integrated_operations
process
strategic
```

`ToolStructure` entrega estructura reutilizada por KPI y por contratos de Alarm baseline.

Esta sección describe el dominio Tool/ToolConfiguration. No describe el generador de aplicaciones ni el project tooling.

## Frontera Tool→Tool no cerrada

No se demostró todavía un contrato explícito para consolidación:

```text
Tool A → Tool B
```

No asumir si B consume Source originales, una Projection de A, una publicación de A o algún contrato distinto.

No inventar ese contrato.

Esta frontera permanece **PLANNED / SEPARATE** y no bloquea la qualification del Starter ADA generado.

## Qualification observada de este hito

Para el incremento de generación/distribución:

```text
33 tests seleccionados                  PASS
Ruff scope del incremento               PASS
sh -n launchers                         PASS
build distribution                      BUILT_UNQUALIFIED
internal wheels                         69
distribution precheck                   PRECHECK_PASS
source_git_head                         a75465745e188da4765e803595b17acaa55d9306
project.sh --help artifact final        PASS
```

No se ejecutaron físicamente los `.cmd` en Windows.

No se calificó todavía el runtime Web final, Docker, Cosmos/Azurite ni el recorrido visual/funcional Home → header → Navigation → Manager sobre el artifact final trazable.

Los conteos históricos de qualification ADA Generic/Manager pertenecen a sus cortes previos y no deben sumarse ni atribuirse automáticamente a `a7546574`.

## Estados CURRENT/CLOSED/OPEN

CLOSED:

```text
Generated Tool Home/pages/modules contract
Generic demo exclusion for ADA profile
Reusable project tooling extraction
Human Web tooling .sh/.cmd contract
Distribution build precheck
Source traceability
```

CURRENT:

```text
ADA Generic composition
Manager integration
ToolConfiguration domain contract observado
69-wheel ADA distribution at a7546574
```

OPEN / UNVERIFIED:

```text
.env.detail audit and system-assignable values
project init/sync on final traced artifact
Cosmos/Azurite boot
Web runtime boot
Home/header/Navigation/Manager E2E
Docker image/runtime qualification
Windows .cmd physical execution
Azure/Entra production
Python 3.14.7 / slim-trixie alignment
```

## Decisiones refinadas en este corte

La aplicación ADA generada ya no se considera un starter que deba copiar un ejemplo Generic para demostrar extensibilidad. La extensibilidad es explícita mediante `application.pages` y `application.modules`.

El project tooling ya no se considera implementación que deba copiarse completa dentro de cada Tool. Su implementación reusable pertenece a un paquete de tooling de distribución y la Tool conserva un bootstrap delgado.

La interfaz humana de los comandos de distribución no es “ejecutar un `.py` directamente”; `.sh` y `.cmd` son las superficies soportadas y el Python queda como implementación.

Estas refinaciones no crean un contrato Tool A → Tool B ni modifican el dominio `ToolConfiguration`.

## Siguiente foco

```text
ADA-GENERATED-APPLICATION-LOCAL-RUNTIME-QUALIFICATION
```

Usar el artifact trazable de `atlanticus@a75465745e188da4765e803595b17acaa55d9306`.

Orden del siguiente frente:

```text
1. auditar/completar .env.detail
2. identificar valores manuales vs asignables por sistema sin inventar configuración
3. preparar Cosmos/Azurite local
4. project init/sync del artifact final
5. levantar Web
6. verificar Home → header → Navigation → Manager
7. recién después calificar Docker/recovery según alcance acordado
```

No mezclar en ese incremento Tool→Tool, KPI E2E, Alarm E2E, Command Center ni la migración Python 3.14.7/trixie salvo que se abra explícitamente otro foco.
