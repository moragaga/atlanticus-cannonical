# Manager — Canonical Index

Estado: **CURRENT / HEADER + ADA LOCAL UI CLOSED / REAL PERSISTENCE QUALIFICATION OPEN**

Checkpoint de este cierre: `moragaga/atlanticus@392ee281a32396516fb08c23c63514d8cbdb3489` (inspección estática de `main`, 2026-09-27). Canonical previo: `moragaga/atlanticus-cannonical@a4c813bf6c833455ebe4f5f0a5968b0633c7b045`. Ver `07_SOURCE_LEDGER.md` para separar evidencia manual, pruebas reportadas e implementación observada.

| Archivo | Contenido | Estado |
|---|---|---|
| `01_APPLICATION_BOUNDARY.md` | Manager como capability independiente. | CURRENT |
| `02_NAVIGATION_AND_HOME.md` | Home, sidebar, header, navegación administrativa y UI Navigation Configuration. | CURRENT |
| `03_WORKFLOW_AND_SESSION.md` | WORKSPACE/SOURCE/PROJECTION y entries administrativos. | CURRENT |
| `04_TOOL_CONFIGURATION.md` | Tool Configuration y contrato Source/Projection. | FROZEN / CURRENT |
| `05_SOURCE_BLOB_HANDOFF.md` | Source/Projection consumido por Manager. | CURRENT |
| `06_TESTING_BOUNDARY.md` | Testing contractual y frontera visual. | CURRENT POLICY |
| `07_SOURCE_LEDGER.md` | Evidencia histórica y cierre local de header/identidades. | AUDIT LEDGER |
| `08_BOOTSTRAP_AND_ACCESS.md` | Bootstrap, acceso y frontera de identidad. | CURRENT |
| `09_ADA_COMPONENT_LINKS.md` | Links externos y warmup. | CONTRACT DESIGN |

## Contratos CURRENT

`ManagerModule` conserva Source/Projection y workflow de su dominio; `ManagerEntry` representa una capability administrativa sin Source/Projection ficticios, por ejemplo Users. `ManagerAuthorizationPolicy.can_view(principal, item)` controla visibilidad; `is_local` no constituye un bypass de permisos. Home y sidebar consumen `ManagerModuleRegistry` con autorización previa. `/manager` es Home propia, no una redirección a un módulo.

El header es genérico: marcas, título/subtítulo y enlace opcional de retorno se inyectan por el consumidor. El Manager core no incorpora identidad gráfica ADA obligatoria ni muestra `principal.display_name` en el header. El principal sigue siendo requerido por autorización y para resolver el contexto de navegación; retirar la etiqueta visual no elimina autenticación.

## Composición ADA CURRENT

```text
Administración
└── Usuarios

Configuraciones
├── Perfiles
├── Accesos
├── Navegación
├── Herramienta
├── KPI
└── Definiciones KPI
```

ADA inyecta las marcas ADA y Atlanticus. La marca de Los Pelambres fue retirada **únicamente de este header y sus assets propios**; no se eliminaron recursos de otras aplicaciones. Los enlaces visibles son `Manager Home` y, cuando la aplicación host lo define, `Volver a la aplicación`. El header no muestra nombre de usuario. `Usuarios` y `Perfiles` son labels inyectados por ADA; la capability genérica Users/Profiles no cambia de nombre.

## Fronteras abiertas, no confundibles con este cierre

- `MANAGER-LOCAL-INTEGRATION`: **CLOSED / VERIFIED LOCAL**.
- `MANAGER-DURABLE-ADAPTER-COMPOSITION` y CLI de preparación: **CURRENT / previamente verificados en tests locales**; **real Blob/Cosmos/restart: OPEN / UNVERIFIED**.
- Header de Manager en ADA Generic **source local**: **CLOSED / VERIFIED MANUAL** por el usuario.
- Header/Manager y navegación HTML autorizada **dentro del Starter ADA distribuido**: **OPEN / UNVERIFIED**. El Starter históricamente se cualificó con Manager `disabled`; la aceptación visual del core no sustituye esa prueba.
- Hallazgo histórico `NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT`: **BLOCKED / SEPARATE**, pendiente de revalidación sobre `main` antes de cualquier cambio.

La siguiente frontera de este chat es **auditar distribución existente**; no se abre diseño de Manager ni de alarmas en este cierre.
