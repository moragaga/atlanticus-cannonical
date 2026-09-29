# ADA Command Center — Current Implementation

Estado: **CURRENT — Source v3/Materialization/Runtime, C1 ownership Web y C2 identidad CLOSED; C4 Delivery CURRENT-only CLOSED en código y regresión local (2026-09-29)**. Golden Path productivo, Docker/Azure, Live y Web operacional permanecen no acreditados.

## Checkpoints

```text
atlanticus:main (C4)    45eff96d777f4711cb011f779ffc0a6c87bf0ca4
atlanticus previo C4   18029e19ff01e58b9c9399c132ff32b5ca913f06
atlanticus-decisions   50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
canonical base C4      2e8bbf4780cafc4cea3b18351861aa97a4fb0053
C1 histórico           3961385aecd0eb7e373018fc25e509a71dccc409
B2c.7 histórico        c67fcb5b105cc561c16719a8bca4ea5aa74c3fae
```

Implementación y HEAD verificados mediante Git de solo lectura. Los conteos de pruebas provienen de logs aportados por el usuario y no equivalen a ejecución remota independiente.

## Componentes existentes

```text
scopes/ada-command-center/
  domain/alarms/                             # Snapshot v3 y Source Key única
  domain/tools/                              # ToolDependencyManifest
  backend/alarms/core/                       # Engine puro
  backend/alarms/materialization/            # resolver B.2 / READY exacto
  backend/alarms/persistence/                # WAL / fencing / EFFECTIVE
  backend/alarms/contracts/                  # CURRENT v1 / FACTS v2
  backend/processes/alarms-materialization/  # Cosmos + qualification manual + READY/BLOCKED
  backend/processes/alarms-runtime/          # adopta EFFECTIVE / CURRENT + FACTS
  backend/processes/alarms-delivery/         # receptor de último CURRENT exclusivamente (C4)
  web/alarms/configuration/
  web/alarms/persistence/
  web/alarms/projection-local/
  web/alarms/projection-cosmos/
  web/tools/catalog/
  web/tools/discovery-cosmos/
  web/tools/catalog-manager/
  web/application/ada-command-center-configuration-manager/ # host temporal
```

`backend/tools` está SUPERSEDED. Domain Tools conserva contrato transversal; los servicios usados exclusivamente por Web viven en Web. Materialization sigue importando `web/alarms/projection-cosmos`: frontera técnica OPEN no modificada en C4.

## C2 preservado — identidad y configuración

`APPLICATION=ada-command-center` identifica los tres jobs, que conservan `job_key` y lease propios (`alarms-materialization`, `alarms-runtime`, `alarms-delivery`). `VOLUMEN_PATH` es manual, absoluta y debe referirse al mismo montaje **físico**. La ruta raíz no cambia: `VOLUMEN_PATH/ada-command-center/alarms`.

`domain/alarms/identity.py` define texto `ALARM_CONFIGURATION_SOURCE_KEY = 'alarm-configuration'`; Web lo convierte a `SourceKey` técnico y los jobs lo consumen como texto. Se verifica la identidad registrada en los documentos, no se confía únicamente en la constante. Materialization toma physical name y partición de entrada del resource contract Web `ALARM_CONFIGURATION_PROJECTION_STORAGE_RESOURCE`; el endpoint/base/credencial Cosmos deben configurarse y verificarse físicamente frente a Web. El contenedor Blob sigue ambiental.

No existen despliegues previos que migrar según confirmación del usuario; sin capas legacy, aliases ni migraciones hipotéticas.

## Pipeline existente: qué cambia C4

1. Source v3 congela `AlarmConfigurationSnapshot(configuration, tool_dependencies)` con referencias Rn/Cn; el Tool Catalog en Blob no se reinterpreta desde latest.
2. Materialization obtiene ProjectionRecord y qualification manual externa; B.2 publica `runtime.json` y `delivery.json` bajo manifest READY íntegro, o diagnóstico BLOCKED sin sustituir READY.
3. Runtime adopta pin exacto `source_key + result_id + manifest_sha256 + resolution_key` usando WAL/EFFECTIVE; READY no activa Engine por sí solo.
4. Runtime publica snapshot completo/reemplazable `runtime/output/current/latest.json` v1 y continúa exportando FACTS v2 encadenados desde commits durables mediante su **cursor productor**.
5. **C4 CURRENT:** `LocalAlarmDeliveryReceiver` lee únicamente el último CURRENT, valida estructura/SHA/source/timestamps, exige identidad exacta con EFFECTIVE y la pareja READY; coloca el snapshot válido en `delivery/input/current/latest.json`. Un mismatch devuelve espera y no incorpora la nueva entrada. Reinicio valida CURRENT persistido; no existe cursor consumidor FACTS nuevo.

El inbox CURRENT anterior puede permanecer físicamente si Engine ha cambiado EFFECTIVE y el último CURRENT aún no coincide. C4 **no publica Live ni despacha operacionalmente**; cualquier materializador/despachador futuro debe imponer la misma igualdad exacta antes de usarlo. No se añadió invalidación, sincronización de relojes, mecanismos de reintento especiales ni lógica especulativa.

## C4 — cambios limitados y evidencia

La comparación de Git entre los SHAs de arriba confirmó **14 archivos modificados únicamente bajo `backend/processes/alarms-delivery`**: `receiver.py`, `job.py`, `settings.py`, `bootstrap.py`, espejos comentados, cuatro tests, `.env.detail` y `secrets.detail.json`.

- Retirados `FACTS` como entrada Delivery, cursor `delivery/input/state/facts-consumption-cursor.json`, límite `ALARM_DELIVERY_MAX_FACTS_PER_ITERATION` y iteration facts derivados de FACTS.
- Preservados pin exacto, lector READY, EFFECTIVE, checksum, estructura, tiempo UTC, prevención de retroceso/conflicto de `as_of`, lease/fence y recuperación de CURRENT.
- `config/connections.json`, `ALARM_DELIVERY_MAX_WORKERS`, `ParallelCosmosPublisher` y gate actual del bootstrap que depende del registro Cosmos permanecieron **sin refactor**. Publisher paralelo no forma parte de la recepción CURRENT en este incremento; revisar si corresponde solo en foco futuro explícito.
- No se cambió producción Runtime de FACTS v2, cursor exportador, WAL, contratos de Source ni Materialization.

**VERIFIED por logs locales de este hito:** ZIP de 14 archivos integrado, `git diff --check`, `uv lock --check`, 29 PASS Delivery, 16 PASS publicadores Runtime, Ruff PASS en archivos C4. `uv sync --locked` inicial removió el paquete workspace Materialization y provocó cuatro errores de colección; `uv sync --locked --all-packages` restauró dependencias, import Materialization PASS y `uv run --locked --all-packages pytest` cerró con **567 PASS, 1 SKIPPED** en **Python 3.14.2**. No trasladar ese resultado a CI, Docker o Azure.

El error Ruff `SIM117` de `tests/test_parallel.py` precedía a C4 en Git y no afecta la comprobación Ruff acotada de los archivos modificados. El baseline global declarado en el Project usa Python 3.14.7 pero este workspace todavía exige 3.14.2: revisión aparte, no refactor dentro de C4.

## OPEN separados

- **C3:** productor/verificadores reales GREEN y qualification más allá del archivo manual.
- **C5:** propietario, key y versión del contrato de evidencia técnica, y auditoría ambiental.
- **Docker/distribución:** ruedas/entrypoints/instalación aislada, tres procesos y volumen realmente compartido; el siguiente foco recomendado se limita primero a Runtime + Delivery.
- **Materialization ↔ Web Projection Cosmos:** dependency owner y equivalencia física cuenta/base quedan abiertos.
- **Live:** `AlarmLiveProjection`, enriquecimiento cause, visibilidad/priority ya decidida, despacho y store final NO implementados.
- **Separados:** Starter Web, Management Capture/Projection, History/Analytics, UI fin de turno/modal, Azure/CI/browser.

No introducir funcionalidad ni alterar código durante este cierre documental.
