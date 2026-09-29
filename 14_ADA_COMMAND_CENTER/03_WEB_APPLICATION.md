# ADA Command Center — Web Application

Estado: **CURRENT — Tool Catalog/Discovery y Alarm Configuration como capabilities Web; host Configuration Manager temporal disponible; UX-01/UX-02 y operación básica de prueba aislada CLOSED; Starter genérico propio con runtime/home PLANNED**. Actualización documental 2026-09-29; **no declara distribución ni aplicación operacional completa**.

## 1. Autoridad y composición actual

```text
Implementation       moragaga/atlanticus:main@2e7500a6b8b4d5bbdad26d807abfa57936db99d5
Decisions            moragaga/atlanticus-decisions:main@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
Canonical previo     moragaga/atlanticus-cannonical:main@2e8bbf4780cafc4cea3b18351861aa97a4fb0053
```

Command Center tiene Web propia; **no** se monta automáticamente como página de `ada-generic-application` ni convierte una capability en un proceso remoto por defecto.

Capabilities existentes:

```text
scopes/ada-command-center/web/
  alarms/configuration
  alarms/persistence
  alarms/projection-local
  alarms/projection-cosmos
  tools/catalog
  tools/discovery-cosmos
  tools/catalog-manager
  application/ada-command-center-configuration-manager  # host TEMPORAL
```

C1 trasladó Catalog/Discovery exclusivamente Web a `web/tools/`; `web/tools/catalog-manager` posee UI/callbacks propios. `backend/tools` está SUPERSEDED, `domain/tools` permanece transversal. El host temporal reutiliza estas bibliotecas, no duplica su implementación ni sustituye automáticamente un Starter.

## 2. Host temporal: dos providers, no tres

El ejecutable existente es `scopes/ada-command-center/web/application/ada-command-center-configuration-manager/src/.../__main__.py`. Compone Alarm Configuration y Tool Catalog en `/manager`, con entry `/tool-catalog` y módulo `/alarm-configuration` bajo el Manager genérico.

Configuración real de `ManagerConfigurationReader`, leída desde el directorio de ejecución y `.env` no productivo:

| `ATLANTICUS_ENVIRONMENT` | `ADA_MANAGER_PERSISTENCE_PROVIDER` | Alarm Source | Alarm Projection | Confirmed Tool Catalog |
|---|---|---|---|---|
| `local` | `local` | Filesystem `<cwd>/.runtime/...` | Filesystem `<cwd>/.runtime/.../projections` | **Blob/Azurite** |
| `local` | `durable` | Blob | Cosmos | **Blob/Azurite** |
| `production` | `durable` | Blob | Cosmos | Blob, host autenticado requerido |

**VERIFIED:** `local` **no** significa «sin emuladores»: el Tool Catalog en ambos modos usa `BlobToolCatalogStore` con `ADA_COMMAND_CENTER_STORAGE_CONNECTION_STRING` y `ADA_COMMAND_CENTER_STORAGE_CONTAINER_NAME`. El host `__main__` rechaza producción sin host autenticado. No introducir tercer modo `sample`/`durable-local`, catálogo de ejemplo o migración sólo para la prueba.

Config Cloud/durable actual: Tool Catalog confirmado en `conciencia_situacional/command-center/tool-catalog/current.json`; Source Alarm en mismo namespace de Blob. `ALARM_CONFIGURATION_SOURCE_KEY` textual proviene de Domain y se transforma a `SourceKey` sólo en Web. Projection Cosmos usa resource contract `ALARM_CONFIGURATION_PROJECTION_STORAGE_RESOURCE` (`ada-command-center-alarm-configuration-projection`, PK `/partition_key`) y binding manual de conexión; Materialization debe apuntar a la misma base/cuenta física, **UNVERIFIED**.

Conexiones externas Tool Cosmos siguen siendo nombradas dinámicamente (`ADA_COMMAND_CENTER_TOOL_COSMOS_<NAME>_{ENDPOINT,DATABASE_NAME,KEY}`). La existencia de datos en ellas no crea automáticamente Tool Catalog: Discovery/inspección/confirmación es intervención humana explícita, Store durable es Blob y el editor requiere catálogo confirmado al guardar workspace.

## 3. Hito de UX — CLOSED acotado

**UX-01:** Save Draft del modal cierra sólo después del guardado exitoso; errores permanecen en modal. **UX-02:** selectores Rule default y Message override ofrecen `1..11` y `END_OF_SHIFT`, con Domain/Materialization compatibles con el nuevo valor estático. En Git: `main@2e7500a...`; el usuario reportó Domain 59 PASS, Materialization 64 PASS, Configuration Web 123 PASS = **246 PASS** y `git diff --check` limpio.

La aceptación funcional básica en browser se realizó con `Atlanticus_CC_test_local_sin_emuladores.py`, un **lanzador de prueba externo** no integrado al repositorio: crea aplicación real con una Source/Projection de filesystem **aisladas** en `.runtime/ux02-ui-test-isolated`, `TestOnlyFileCatalog` con una sola Tool Process sintética, sin Azurite/Cosmos y sin entrada Tool Catalog Manager (`tool_catalog_manager=None`). Publica CSS/JS de las capas reales y comprueba HTTP/página/callbacks/recursos antes de servir en localhost. El usuario pudo abrir, crear familias, Rules/Messages, asignar y guardar.

**No promover ese lanzador a Starter:** el fixture no es un productor de Tool real, no prueba descubrimiento Cosmos, no simula qualification GREEN de evaluator, no valida ejecución Engine ni demuestra recuperación/persistencia durable.

## 4. Findings vigentes de Web — OPEN

- Usuario observó valores de campos que desaparecen, validaciones que dejan de mostrarse y avisos success/warning/danger pegados. La causa no se aisló. Aunque no bloquearon creación/asignación/guardado en el ensayo, pueden comprometer la integridad de drafts; **no** hay aceptación productiva de UX.
- Selección manual/round-trip de `END_OF_SHIFT` y recuperación de releases tras reiniciar la aplicación: **UNVERIFIED** fuera de tests de componente/codec.
- La cadena operacional «fin de turno»: Web que conoce horario del proceso/turno debe proporcionar un `effective_until` UTC confiable; el editor sólo guarda la política estática, Core no calcula calendarios. **PLANNED**.
- Entorno nativo sin Azurite, startup sin datos y fallo aislado de capabilities en el **nuevo** Starter: por acordar; no heredar `TestOnlyFileCatalog`.

## 5. Starter genérico propio — PLANNED / siguiente frontera exclusiva

El nombre `ada-command-center-generic` aparece como **denominación objetivo previa en canonical**, pero **package exacto, path, entrypoint, composición y distribución requieren auditoría en Git y decisión**, no están implementados por nombrarlos. No acoplar a `ada-generic-application`, no construir un Manager específico nuevo ni trasladar lógica de tools, alarms o runtime al host.

**Dirección seleccionada por el usuario:** un Starter de Command Center capaz de levantar su runtime y **una página de inicio mínima**, suficiente para instrumentar un flujo real que ejecute alarmas. Excluir inicialmente Navigation, Users, Profiles, dashboard completo, Historia/Explorer y presentación visual `CAROUSEL`/`QUEUE_IN_QUEUE`. Un host de prueba o shell accesible no equivale a Live implementado.

La fase inicial del próximo chat debe inspeccionar fronteras existentes y especificar **qué demuestra exactamente el gate**:

```text
Tool Source/Projection real + confirmación Cn
  -> Alarm Source Rn/Cn y Projection
  -> Materialization con qualification manual controlada si aplica
  -> READY exacto Runtime/Delivery
  -> Runtime WAL/EFFECTIVE + Engine CURRENT v1 y FACTS v2
  -> Delivery CURRENT-only receptor
  -> [PLANNED] Live materializer/AlarmLiveProjection si la home
     ha de mostrar alarmas operacionales resueltas
```

**No construir nuevos contratos de datos, API Live, adaptadores ni código de ejemplo productivo** para fingir que existe el tramo final. Primero identificar qué ya está integrado y qué es dependencia pendiente; proponer sólo el incremento mínimo verificable después de debate.

## 6. Shell, identidad y módulos futuros — límites congelados

El shell final será propio de Command Center y podrá **componer** primitives reutilizables y `atlanticus.web.manager`, sin heredar automáticamente el header operacional de ADA ni el header Manager ADA. Navigation, Users, Profiles y actividad de usuario no pertenecen a esta primera frontera; Microsoft Entra ID se incorpora en host productivo cuando el contrato lo requiera, sin segundo sistema de identidad.

Separar refresco de aplicación/sesión de Live/Analytics y no copiar `auto-refresh` de ADA Generic por analogía. Si User Activity se retoma más tarde, su integración transversal es opt-in con contrato previo de visitas/TTL. El Starter deseado debe distinguir ausencia de datos, indisponibilidad y configuración inválida, degradando sólo las capabilities afectadas cuando su composición lo permita; sus comportamientos concretos permanecen por verificar.

No poner reglas/evaluadores/calendarios de deactivation en UI como autoridad operacional. Web no recalcula priority/routing ni materializa `cause_template`; Web tampoco lee WAL o usa FACTS como API Live.

## 7. Documentación y gates pendientes

Este reemplazo actualiza el **estado Web/UX** y la **frontera futura**; no acredita el gate físico Blob↔Cosmos de Alarm Source/Projection, Docker jobs independientes, misma ruta de volumen físico, distribución limpia ni Live Delivery. `atlanticus-cannonical:main@2e8bbf...` aún describe C4 PLANNED en otras páginas mientras Git ya lo implementó: integrar/reconciliar los reemplazos C4 de otro cierre **antes** de declarar canonical totalmente sincronizado.
