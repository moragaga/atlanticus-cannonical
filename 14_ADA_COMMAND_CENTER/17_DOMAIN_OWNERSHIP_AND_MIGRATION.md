# ADA Command Center — Domain Ownership and Migration

Estado: **CURRENT — C1 Tool owners Web CLOSED; C2 Source Key única Domain y consumidores Web/Backend CLOSED estructuralmente; frontera técnica Projection Cosmos pendiente aparte**. Checkpoint: `atlanticus:main@18029e19ff01e58b9c9399c132ff32b5ca913f06`.

## Domain Alarms — CURRENT / delta C2

```text
scopes/ada-command-center/domain/alarms
ada-command-center-alarms-domain==1.0.0
```

El Domain posee el contrato de authoring `AlarmConfiguration`, `AlarmConfigurationSnapshot` v3 y ahora la identidad transversal **en texto plano** `ALARM_CONFIGURATION_SOURCE_KEY = 'alarm-configuration'`, declarada en `identity.py` y exportada desde `__init__.py`. La constante no conoce `SourceKey` de Web, Cosmos, Blob ni paths de jobs. El host Web realiza `SourceKey(ALARM_CONFIGURATION_SOURCE_KEY)` en su composición; Materialization, Runtime y Delivery importan el mismo texto y lo exponen desde sus settings.

Esta extensión C2 tiene frontera transversal real porque la misma identidad es verificada en Source/Projection, READY, Runtime EFFECTIVE y Delivery; no es un motivo para trasladar infraestructura al Domain.

## Domain Tools — CURRENT desde antes de C2

```text
scopes/ada-command-center/domain/tools
ada-command-center-tools-domain==1.0.0
ToolDependencyEntry
ToolDependencyManifest
```

Los tipos transversales Tool manifest permanecen independientes de consolidación física. La dirección de dependencias declarada después de C1 persiste: contracts `ada-web-tools` → Domain Tools → snapshot wrapper Domain Alarms. `domain/alarms` **no** tiene dependencias vacías. Una normalización futura de tipos estructurales actualmente residentes en `ada-web-tools` sigue diferida; C2 no la realizó.

## Tool services Web — C1 CLOSED

```text
web/tools/catalog            -> snapshots, consolidación y Blob CURRENT
web/tools/discovery-cosmos   -> conexiones nombradas, inspect/confirm/adopted
web/tools/catalog-manager    -> UI/callbacks capability-local
```

`backend/tools` y namespaces/packages antiguos están SUPERSEDED. El host temporal Web compone services y capability UI; no replica su lógica. Una función server-side escrita en Python y usada solo por Web sigue perteneciendo a Web; Domain no absorbe adapters por mera cercanía.

## Backend Alarm y frontera no resuelta

```text
backend/alarms/materialization          -> resolver B.2, codec/lector READY exacto
backend/alarms/core                     -> Engine puro
backend/alarms/persistence              -> WAL/EFFECTIVE/fencing
backend/processes/alarms-materialization
backend/processes/alarms-runtime
backend/processes/alarms-delivery
```

C2 unificó el valor **ambiental** `APPLICATION=ada-command-center` en los tres procesos, manteniendo `service_name`/`job_key` propios. La ruta `VOLUMEN_PATH` sigue siendo entrada absoluta definida por el operador; las raíces durables existentes permanecen bajo `VOLUMEN_PATH/ada-command-center/alarms`. No hay legado que migrar: el usuario confirmó que no existía despliegue previo.

**Dependencia OPEN:** `backend/processes/alarms-materialization` aún importa adaptador y resource contract de `web/alarms/projection-cosmos`. C2 mantuvo esa dependencia actual y eliminó sólo el nombre físico Cosmos ambiental duplicado, consumiendo `ALARM_CONFIGURATION_PROJECTION_STORAGE_RESOURCE` para physical name/partition. Una futura resolución de ownership requiere alcance propio y contrato neutral real, no mover todo a Domain ni crear shims.

## Web Alarm Source/Projection — CURRENT

`web/alarms/configuration` posee authoring, Source/Release y workflows; `web/alarms/persistence`, `web/alarms/projection-local` y `web/alarms/projection-cosmos` contienen adapters existentes. `web/application/ada-command-center-configuration-manager` compone cliente, principal, provider y binding Cosmos durable.

El physical name `ada-command-center-alarm-configuration-projection` y su partición `/partition_key` proceden del resource contract Web actual; Web resuelve connection ref mediante la topología existente, Materialization lee las propiedades contractuales. **NO** significa que ambas aplicaciones apunten automáticamente a la misma cuenta/base: Materialization mantiene `ALARM_COSMOS_ENDPOINT`, `ALARM_COSMOS_DATABASE_NAME` y credencial por despliegue. El physical name del contenedor Blob de Web permanece configurable. Delivery posee registry de conexiones Cosmos independiente; no derivarlo del input Alarm Projection.

## Fronteras siguientes sin código C2 adicional

- **C3 PLANNED / BLOCKED:** definir productor/verificadores qualification GREEN reales; la intervención humana manual sigue admitida.
- **C4 PLANNED:** sustituir el consumo CURRENT+FACTS del receiver Delivery por latest CURRENT solamente; **mantener** producción Runtime FACTS v2 y su WAL.
- **C5 PLANNED:** resolver evidencia técnica y completar auditoría ambiental restante con contrato y propietario reales.
- **SEPARATE:** Starter propio `ada-command-center-generic`, Live materializer, Management Capture, History/Analytics, Docker/Azure y aceptación Web visual.

## No legacy / no inferencia

No restaurar `backend/tools`, convertir Domain en infraestructura, forzar migraciones sin estado real, crear alias de Source, introducir nombres físicos Cosmos en `.env`, inferir rutas/montajes del desarrollador, suprimir FACTS antes de C4 ni adjudicar implementación Live por el receptor actual. Conservar separación de ownership y cumplir etapa de debate contractual antes de nuevas modificaciones.
