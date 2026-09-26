# Web Platform — Source Ledger

Estado: **AUDIT LEDGER / HISTORICAL EVIDENCE PRESERVED / CURRENT CHECKPOINT ADDED**

## Corte histórico anterior (NO CURRENT)

```text
moragaga/atlanticus@29bbf6d8f2b47a7d31e967ad4bb8de42f67a4c85
Parent: 856498c52f182cd531deae845c25bd51ae2ff4ea
Tree: 3f27ad599c6dec610dff5317494a73b276d2ebc4
```

### Users — contratos mantenidos

Ownership `identity + lifecycle + user -> profile_key`. No introducir `authority_key`, `users/configuration`, Users generic Projection ni Users Manager Source/Projection module.

### Profiles — contratos mantenidos

```text
profiles/core
profiles/configuration
profiles/projection-local
profiles/projection-cosmos
web/compositions/profiles-manager
```

Profiles es generic Atlanticus y posee `ProfileCatalog`.

### ADA Access — contratos mantenidos

`profile_key -> access_keys`. ADA Access Configuration Web/Manager está implementado; una afirmación histórica de ausencia de superficie Web está SUPERSEDED.

### Navigation — contratos mantenidos

```text
Navigation Configuration -> Profiles core: REMOVED
Navigation Configuration -> Users: FORBIDDEN
Navigation Configuration -> ADA: FORBIDDEN
Navigation -> Users: FORBIDDEN
Navigation -> ADA Access: FORBIDDEN
```

Los contratos neutrales `NavigationProfileOption` / `NavigationProfileOptionsProvider` permiten adaptar Profiles desde ADA sin dependencia del core Navigation. `allowed_profiles=()` es público dentro de la autorización Navigation; valores no vacíos restringen visibilidad. ADA excluye `root/local` del selector de Navigation. Su UI anterior cerró: link editor como único lugar de asignación de perfiles, paginación top-level 10/20, expansión efímera y correcciones visuales previamente aceptadas.

### Manager — contratos mantenidos

Manager distingue `ManagerModule` y `ManagerEntry`. ADA Configuration Manager compone Users (Administración) y Profiles, Access, Navigation, Tools, KPI, KPI Definition (Configuraciones).

Qualification histórica previa al último patch visual:

```text
22 Navigation core passed
42 Navigation Configuration passed
10 Navigation Manager passed
5 ADA Configuration Manager focused passed
```

No reutilizar esas cifras como qualification del nuevo checkpoint ni mezclar otros frentes. La antigua recomendación «siguiente página: Herramienta» corresponde a UI histórica y queda SUPERSEDED como foco actual de este chat.

El finding histórico `NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT` (`can_view` frente a `can_access`) permanece **BLOCKED / SEPARATE**; no añadir alias.

## Nuevo checkpoint ADA Generic — 2026-09-24

```text
moragaga/atlanticus@ce1213ec14cdee0be905c042c1cf513d71fb5b2d
Parent inmediato observado: 842d9fa7a6ac8b96d0e021b8f349e0a56c26f055
Base canonical inspeccionada: 6bd7f1f2616f954b422f3ddc1549a53a9b479682
```

**VERIFIED:** inspección remota de los archivos actuales, comparación de commits, código de composición y topología, `.env.detail`; el usuario ejecutó tras la corrección 1H.1 `uv lock --check`, Ruff check/format, **157 tests**, validación de mirrors y `uv build --wheel` con resultados satisfactorios. La secuencia de 1G.2 había pasado 140 tests; queda SUPERSEDED como evidencia de versión final por la qualification local posterior de 157 tests.

La secuencia implementada en este frente fue:

```text
1F: bootstrap Identity/Users compartidos y Manager local
1G: adaptadores durables Blob/Cosmos
1G.1: consolidación Profiles + Access en users-support
1G.2: Navigation fuera de users-support en navigation-projection
1H: selector, composición durable local y comandos de preparación Cosmos/Blob
1H.1: nombres Cosmos internos, fuera de .env; Blob container configurable
```

`1F/1G/1H` son etiquetas de **incrementos**, no temporizadores ni contratos de retención. Los estados intermedios 1G.1 y configuración anterior a 1H.1 quedan **SUPERSEDED** por el código CURRENT.

Configuración CURRENT: `ADA_MANAGER_PERSISTENCE_PROVIDER=auto|local|durable|disabled`; `durable` exige Source Blob + Projection Cosmos de Tool en esta etapa y reutiliza una conexión/base Cosmos ADA; el contenedor Blob físico se configura con la connection string/SAS correspondiente. Cosmos usa seis contratos internos del plan Manager, con Navigation independiente y Profiles/Access en `users-support`. No hay nombres de contenedores Cosmos en `.env` de ADA Generic.

**UNVERIFIED:** ejecución real de CLI `ensure-local|validate`, publicación/proyección contra proveedores reales, reinicio, recuperación, imagen Docker, identidad productiva. **VERIFIED STATIC / IMPACT UNVERIFIED:** `bootstrap` crea un `AccessRuntime` distinto del registrado por `create_identity_module`; requiere qualification de coherencia de identidad/Manager.

## Siguiente frontera después del checkpoint

```text
ADA-GENERIC-DOCKER-REAL-PERSISTENCE-QUALIFICATION
PLANNED / NEXT
```

No introducir nuevos frentes mientras se registra este cierre. Mantener la separación de lo localmente probado respecto de infraestructura real y de CI transversal.
