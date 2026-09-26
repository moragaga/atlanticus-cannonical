# Manager — Canonical Index

Estado: **CURRENT GENERIC CORE / ADA FINAL ADMIN COMPOSITION + DURABLE ADAPTER COMPOSITION IMPLEMENTED / REAL PERSISTENCE QUALIFICATION OPEN**

| Archivo | Contenido | Estado |
|---|---|---|
| `01_APPLICATION_BOUNDARY.md` | Manager como capability independiente. | CURRENT |
| `02_NAVIGATION_AND_HOME.md` | Home, sidebar, navegación administrativa y UI de Navigation Configuration. | CURRENT |
| `03_WORKFLOW_AND_SESSION.md` | WORKSPACE/SOURCE/PROJECTION para módulos y frontera de entries administrativos. | CURRENT |
| `04_TOOL_CONFIGURATION.md` | Tool Configuration y contrato Source/Projection. | FROZEN / CURRENT |
| `05_SOURCE_BLOB_HANDOFF.md` | Source/Projection consumido por Manager genérico. | CURRENT |
| `06_TESTING_BOUNDARY.md` | Testing contractual y frontera visual. | CURRENT POLICY |
| `07_SOURCE_LEDGER.md` | Fuente histórica y checkpoint ADA Generic/Manager durable. | AUDIT LEDGER / UPDATED |
| `08_BOOTSTRAP_AND_ACCESS.md` | Bootstrap, Manager Access y cierres previos de UI. | CURRENT |
| `09_ADA_COMPONENT_LINKS.md` | Links externos y warmup. | CONTRACT DESIGN |

## Autoridad y corte actual

```text
Implementación inspeccionada:
moragaga/atlanticus@ce1213ec14cdee0be905c042c1cf513d71fb5b2d

Canonical base del reemplazo:
moragaga/atlanticus-cannonical@6bd7f1f2616f954b422f3ddc1549a53a9b479682
```

El checkpoint anterior `df5b99502265758e873e0565abf2176cc617104b` corresponde a una qualification histórica de UI Profiles; no representa el código CURRENT de este frente. Conservar su evidencia en el ledger.

## Contratos Manager CURRENT

`ManagerModule`: capability administrativa con Source/Projection y workflow de publicación/proyección.

`ManagerEntry`: capability administrativa visible en el mismo shell, con navegación, routing y autorización, **sin** Source/Projection ficticios. Users sigue siendo `ManagerEntry` y conserva promoción individual y actualización inmediata.

`ManagerAuthorizationPolicy.can_view(principal, item)` es el contrato genérico vigente. No existe bypass de autorización sólo por `principal.is_local` ni por tener un profile administrador. La confianza local de ADA Generic se restringe al proveedor/identidades locales aprobados en el entorno local; no transfiere permisos implícitos a producción.

## ADA Configuration Manager CURRENT

```text
Administración
└── Users

Configuraciones
├── Perfiles
├── Accesos
├── Navegación
├── Herramienta
├── KPI
└── Definiciones KPI
```

Perfiles sigue siendo generic Atlanticus; sólo ADA utiliza la etiqueta visible `Perfiles`. Profiles conserva UI propia, modal de edición de capability, paginación 10/20 y etiquetas Source/Projection inyectables con defaults genéricos. ADA Access permanece application-specific: `root/local` sin restricciones de perfil dentro del modelo Access; `basic/guest/custom` mediante grants explícitos conforme al contrato de ese dominio. Navigation generic no depende de Profiles; la composition ADA le suministra opciones de perfiles mediante provider neutral.

Los cierres UI anteriores para Navigation, Access, Profiles y Users se conservan; pulido responsive y limpieza de tests permanecen diferidos sin reabrirse por esta qualification.

## Nuevo estado de persistencia ADA Generic

```text
MANAGER-LOCAL-INTEGRATION
CLOSED / VERIFIED LOCAL / CURRENT

MANAGER-DURABLE-ADAPTER-COMPOSITION
CLOSED / VERIFIED LOCAL / CURRENT

MANAGER-RESOURCE-PREPARATION-CLI
CLOSED / VERIFIED LOCAL CONTRACT / CURRENT

MANAGER-REAL-PERSISTENCE-QUALIFICATION
PLANNED / NEXT / UNVERIFIED
```

Manager durable reutiliza el Source `blob` y Projection `cosmos` de Tool **en este incremento**, sin convertirlo en una dependencia del Manager core genérico. El plan parcial tiene seis contenedores Cosmos internos en una conexión/base ADA y un Blob físico configurable para Source global, Source por Tool y Users Registry; Navigation usa `navigation-projection`, Profiles/Access usan `users-support` y Users promovidos usan `users-runtime`.

La elección `auto|local|durable|disabled` controla el modo de Manager, **no la retención de datos**. El CLI local usa Jane/John; el productivo exige proveedor externo real para habilitar Manager durable y no lo inventa.

**VERIFIED LOCAL:** 157 tests de ADA Generic, Ruff, mirrors y wheel tras 1H.1. **UNVERIFIED:** infraestructura Docker/Azure real, publicación/proyección/recovery y autorización end-to-end. Ver `15_WEB_PLATFORM/09_CURRENT_GAPS.md` y `15_WEB_PLATFORM/11_OPEN_ITEMS.md`.

## Finding separado

`NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT` permanece **BLOCKED / VERIFIED CONFLICT / SEPARATE** (`can_view` frente a `can_access`). No añadir shim ni alias durante la qualification de infraestructura.
