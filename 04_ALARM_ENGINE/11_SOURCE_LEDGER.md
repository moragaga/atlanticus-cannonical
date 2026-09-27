# Alarm Engine — Source Ledger

Estado: **AUDIT LEDGER / ALARM MATERIALIZATION CHECKPOINT 2026-09-27**

## Autoridad verificada por lectura, sin escrituras

```text
Implementation HEAD : moragaga/atlanticus@b600ca591b56d0924aed752dfae6e9fab2c6f1d6
Canonical baseline  : moragaga/atlanticus-cannonical@772d15078c97802d58d8b658b0d5d5b928fa2ed5
Historical decisions: moragaga/atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
```

Esta auditoría verifica la frontera tratada; no certifica todo Atlanticus ni todos los documentos históricos. El usuario había informado el mismo HEAD y se corroboró por Git en modo lectura.

## Rutas CURRENT comprobadas

```text
scopes/ada-command-center/backend/alarms/materialization/
  src/.../resolver.py
  src/.../qualification.py
  src/.../runtime.py
  src/.../delivery.py

scopes/ada-command-center/backend/processes/alarms-materialization/
  pyproject.toml                        # v0.2.1, requiere ==3.14.2
  src/.../__init__.py
  src/.../__main__.py
  src/.../acquisition.py
  src/.../bootstrap.py
  src/.../candidate.py
  src/.../codec.py
  src/.../composition.py
  src/.../job.py
  src/.../publication.py              # CosmosResultStore de salida: será reemplazado
  src/.../qualification.py            # proveedor manual JSON
  src/.../settings.py
  tests/test_acquisition.py
  tests/test_executable_process.py
  tests/test_commented_mirror.py

scopes/ada-command-center/web/alarms/configuration/
scopes/ada-command-center/web/alarms/projection-local/
scopes/ada-command-center/web/alarms/projection-cosmos/
scopes/ada-command-center/backend/processes/alarms-runtime/
```

En main, `composition.py` construye `CosmosClient` y lo usa tanto para **input** como para `CosmosAlarmMaterializationResultStore` de **output**. `job.py` fija candidate/evidence, ejecuta pure B.2 y revalida antes de publicar. `publication.py` almacena en un documento Cosmos `runtime`, `delivery`, `manifest`, `findings`, más hashes. `__main__.py` habilita ejecución real del módulo pero el entorno Cosmos no se ha probado.

## Evidencia observada del usuario — ANTERIOR a la corrección

Salida proporcionada sobre v0.2.0, 2026-09-27:

```text
uv sync --python 3.14.2   PASS / 48 paquetes instalados
uv run pytest -q          11 FAILED (test_acquisition por fixture tool-a inválida),
                          19 PASSED visibles por el resumen de progreso
uv run ruff check .       5 findings I001 de ordenación de imports
uv run ruff format --check . 10 archivos a reformatear
uv run python -m compileall -q src   sin salida de error visible
uv build --wheel          PASS / wheel 0.2.0 generado
```

El contrato real `ada.web.tools.validation.require_key` usa patrón `^[a-z][a-z0-9_]*$`; `tool-a` es inválido. La versión corregida 0.2.1, ahora visible en HEAD, usa fixture `tool_a` y trae ajustes de formato/metadata. **UNVERIFIED:** no se aportó prueba reproducida del proceso 0.2.1 completo sobre dependencias reales tras publicarlo; no derivar GREEN de los tests antiguos o de la existencia del wheel 0.2.0.

La generación previa en sandbox comunicó 30 pruebas con dobles y comprobaciones estáticas; esa evidencia es **limitada a harness**, no E2E ni validación del repo con dependencias reales.

## Genealogía conservada

```text
Pure B.2 inicial                         9398786ae9af7c00de1bcca9d7a311fe9ef2155f
Command Center Tools domain              9b9600ae96c9153cf70d0fb401905963b8583c2f
Alarm Source v3                           d2a5e14822d3711e64668b8e70cfa15d7ddae2f
Strict routing completo                   411aea44ac60c09d2b07ce41d34c3f378788b97b
Materialization executable + fix en HEAD b600ca591b56d0924aed752dfae6e9fab2c6f1d6
```

Los resultados de las suites antiguas de Alarm Domain, pure Materialization y Web Alarm Configuration, al igual que las campañas R3.5, siguen siendo evidencia **histórica del corte donde se ejecutaron**, no del nuevo job ni de su arquitectura local aún no implementada.

## Límites explícitos

No se verificó infraestructura real Cosmos/Blob, productor GREEN/Evaluator, publicación atómica en volumen, lectura local por Engine/Delivery, Runtime Adoption global ni Live Delivery. Los documentos de reemplazo del ZIP son propuestas documentales preparadas localmente; no constituyen un commit ni reemplazan automáticamente canonical hasta su integración autorizada por el usuario.
