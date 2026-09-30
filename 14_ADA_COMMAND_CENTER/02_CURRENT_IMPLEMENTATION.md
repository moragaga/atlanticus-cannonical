# ADA Command Center — Current Implementation

Estado: **CURRENT — Source v3/Materialization/Runtime, C1 ownership Web, C2 identidad y C4 Delivery CURRENT-only CLOSED; Alarm Configuration projection physical naming alineado entre local y Cosmos en `atlanticus@fe606cbefb932211b8329df9285004f4933df41d`.** Golden Path productivo, Resource Preparation/startup gate, Docker/Azure, Live y Web operacional permanecen no acreditados.

## Checkpoints

```text
atlanticus:main actual   fe606cbefb932211b8329df9285004f4933df41d
atlanticus C4            45eff96d777f4711cb011f779ffc0a6c87bf0ca4
atlanticus-decisions     50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
canonical previo         c2f442b523fb4429f5a8a76c1e6919687016773f
```

Implementación y HEAD fueron verificados mediante Git de solo lectura. Los conteos de pruebas de este cierre provienen de ejecución local aportada por el usuario; no equivalen a CI, Docker ni Azure.

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
  backend/processes/alarms-delivery/         # receptor de último CURRENT exclusivamente
  web/alarms/configuration/
  web/alarms/persistence/
  web/alarms/projection-local/
  web/alarms/projection-cosmos/
  web/tools/catalog/
  web/tools/discovery-cosmos/
  web/tools/catalog-manager/
  web/application/ada-command-center-configuration-manager/ # host temporal
```

`backend/tools` está SUPERSEDED. Domain Tools conserva contrato transversal; los servicios usados exclusivamente por Web viven en Web. Materialization sigue importando `web/alarms/projection-cosmos`: frontera técnica OPEN heredada y no modificada en este hito.

## Identidad y persistencia de Alarm Configuration — CURRENT

`domain/alarms/identity.py` mantiene `ALARM_CONFIGURATION_SOURCE_KEY = 'alarm-configuration'`. Web lo convierte a `SourceKey` técnico y los jobs lo consumen como texto; los documentos persistidos siguen verificando esa identidad.

El nombre físico de la proyección de Alarm Configuration quedó simplificado y compartido entre adapters:

```text
logical_id       ada.command_center.alarms.configuration.projection
physical_name    alarm-configuration
Cosmos PK        /partition_key
```

La autoridad del nombre físico es `ALARM_CONFIGURATION_PROJECTION_PHYSICAL_NAME = 'alarm-configuration'` en `web/alarms/configuration/resources.py`. El resource contract Cosmos reutiliza esa identidad física; no repite `ada-command-center` ni `projection` porque la aplicación ya constituye su propio boundary de Cosmos.

En local, `AdaStorageNamespace('conciencia_situacional', 'command-center')` conserva el mismo namespace lógico y la proyección se materializa en:

```text
<base_root>/conciencia_situacional/command-center/projections/alarm-configuration/
```

En durable, el container Cosmos correspondiente es:

```text
alarm-configuration
```

Este hito **no** implementó una estrategia local completa para todos los recursos de Command Center. Tool Catalog continúa usando Storage incluso cuando `ADA_MANAGER_PERSISTENCE_PROVIDER=local`.

## C2 preservado — procesos y volumen

`APPLICATION=ada-command-center` identifica Materialization, Runtime y Delivery, que conservan `job_key` y leases propios. `VOLUMEN_PATH` continúa manual, absoluta y debe referirse al mismo montaje físico. La raíz operacional continúa `VOLUMEN_PATH/ada-command-center/alarms`.

Materialization toma nombre físico y partición de entrada del resource contract `ALARM_CONFIGURATION_PROJECTION_STORAGE_RESOURCE`. Endpoint/base/credencial Cosmos deben seguir coincidiendo físicamente con el host durable; esa equivalencia E2E permanece UNVERIFIED. El contenedor Blob de Source sigue ambiental.

No existen despliegues previos que migrar según confirmación del usuario; no introducir aliases, fallback ni capas legacy para el nombre anterior.

## Pipeline CURRENT preservado

1. Source v3 congela `AlarmConfigurationSnapshot(configuration, tool_dependencies)` con referencias Rn/Cn.
2. Materialization obtiene ProjectionRecord y qualification manual externa; B.2 publica `runtime.json` y `delivery.json` bajo READY íntegro, o diagnóstico BLOCKED sin sustituir READY.
3. Runtime adopta pin exacto `source_key + result_id + manifest_sha256 + resolution_key` mediante WAL/EFFECTIVE.
4. Runtime publica CURRENT v1 completo/reemplazable y FACTS v2 durables en canal separado.
5. Delivery consume únicamente el último CURRENT, exige igualdad exacta con EFFECTIVE y READY y persiste su inbox CURRENT. C4 no produce Live.

Este hito no alteró ninguno de esos contratos.

## Evidencia de este hito

Commit integrado:

```text
fe606cbefb932211b8329df9285004f4933df41d
refine alarm configuration resource naming
```

Cambios limitados a Alarm Configuration Web, Projection Cosmos y host Configuration Manager temporal, más `.env.detail` y tests/espejos correspondientes.

**VERIFIED local:**

```text
Alarm Configuration Web                         123 PASS
Alarm Projection Cosmos                           5 PASS
ADA Command Center Configuration Manager         28 PASS
TOTAL                                            156 PASS
git diff --check                                 PASS
```

La ejecución global de pytest que produjo cientos de errores de collection fue descartada: había recolectado múltiples paquetes del monorepo fuera de su contexto. Las tres suites aisladas anteriores son la evidencia válida del incremento.

## OPEN separados

- **Resource Preparation + startup gate:** PLANNED como siguiente foco único; no implementado en este hito.
- **Tool Catalog local:** NOT IMPLEMENTED; el host local todavía requiere Storage para el catálogo.
- **C3:** productor/verificadores GREEN y qualification más allá del archivo manual.
- **C5:** owner/key/version del contrato de evidencia técnica y auditoría ambiental.
- **Docker/distribución:** artefactos, entrypoints y recursos físicos siguen UNVERIFIED.
- **Materialization ↔ Web Projection Cosmos:** misma cuenta/base física continúa UNVERIFIED.
- **Live:** contrato acordado en Project, NOT IMPLEMENTED.
- **Management Capture/Projection, History/Analytics:** PLANNED y separados.
- **UX y END_OF_SHIFT operacional:** mantienen sus OPEN contractuales previos.
- **Python:** Project baseline 3.14.7 frente a metadata/tooling todavía 3.14.2; OPEN fuera de este incremento.

No introducir funcionalidad adicional al integrar esta actualización documental.
