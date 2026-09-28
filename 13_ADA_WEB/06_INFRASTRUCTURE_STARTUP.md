# ADA Web — Infrastructure Startup

Estado: **CURRENT BASE / RESOURCE PREPARATION 001 LOCAL VALIDATED / COLD-START DEGRADED CASE OPEN**  
Código vigente leído: `atlanticus@da75752e87036b8318f38f8d405c55e8cb18717d`; resultados manuales de Docker aportados por el usuario el 2026-09-28.

## Contrato

La base Web puede existir sin Tool Source vigente, sin Tool Projection, sin KPI, sin Latest/Timeseries y con capabilities externas transitoriamente indisponibles. Esto es intención contractual; no confundirla con prueba de respuesta instantánea de cada worker en todo escenario.

```text
APPLICATION EXISTENCE != TOOL CONFIGURATION EXISTENCE
APPLICATION EXISTENCE != EXTERNAL RESOURCE AVAILABILITY
APPLICATION EXISTENCE != BUSINESS DATA AVAILABILITY
```

## Bootstrap / persistence CURRENT

```text
AdaGenericSettings
→ provider/client settings
→ ToolPersistenceComposition
→ resolve_operational_tool_projection()
→ Tool Projection durable, si existe
→ WebApplicationDefinition
```

Estados: `READY`, `UNCONFIGURED`, `UNAVAILABLE`, `INVALID`. No hay fallback a Source ni adaptador legacy. El cliente de la resolución Tool tiene lifecycle corto y se cierra; el cliente KPI Delivery pertenece a otra frontera. Si Tool no está `READY`, se materializa la definición base, pero **no** se incorpora automáticamente un collector de una Tool que llegue mucho después.

`ToolStructure` aporta identificadores/bindings, no crea la visualización concreta. No interpretar una definición base existente como garantía de que todos los componentes predefinidos aparezcan ya con error.

## Collector lifecycle — CURRENT

Un collector/cache y un poller por worker cuando Tool es válida y KPI Delivery está configurado. Poller empieza en la primera solicitud operacional elegible; `/health/*`, `/assets/*` y `/.auth/*` no lo arrancan. Browser callbacks leen cache, no Cosmos inline.

Documento KPI faltante al inicio → `MISSING` y stores iniciales vacíos; documento faltante después de un estado bueno → conservar último bueno. Fallos de lectura reportados/reintentados por el poller sin sustituir datos válidos. La recuperación end-to-end con Tool y delivery reales tras caída de Cosmos **no se ensayó**.

## Resource Preparation / Compose — CURRENT y localmente validado

```text
ada-generic-manager-resources prepare
ada-generic-manager-resources validate
```

`prepare` local crea Blob, base Cosmos y seis contenedores del Manager cuando faltan. `validate` no crea. En producción Blob queda `SKIPPED`, se valida base Cosmos preexistente y se crean únicamente contenedores faltantes. `full.yaml` ya no hace depender Web del resultado del job `resources`; ambos procesos usan los dos emuladores como dependencias de `service_started`. La validación individual permanece ajena al ciclo de vida ordinario del proceso Web.

El usuario verificó con Docker el entorno vacío (`CREATED` ×8), reutilización/reinicios (`READY` ×8), fallo Cosmos (`PARTIAL`: Blob `READY`, Cosmos `FAILED`, seis `BLOCKED`), salida 2 cuando ni siquiera pudo iniciarse preparación y recuperación de topología tras restablecer Cosmos. La Web previamente levantada mantuvo `/health/live` 200 durante la caída.

## Finding de cold start y readiness — OPEN

Al recrear Web con Cosmos ya detenido, Gunicorn anunció workers, pero `/health/live` no respondió en la ventana de diez segundos. Tras reiniciar Cosmos, **sin recrear de nuevo la Web**, se observaron `/health/live` y `/health/ready` 200. El cuerpo de readiness fue `checks: {}`; por tanto ese resultado prueba la respuesta del endpoint, no preparación de Cosmos ni Tool.

**No afirmar CLOSED para cold start con Cosmos inaccesible.** La consulta inicial de Tool Projection es síncrona en el bootstrap actual y es un candidato explicativo; no hay medición individual de timeout/stack de cada worker. No confundir el fenómeno con el job de preparación, que ya es independiente.

## Home, visualización y datos — frontera posterior

Requisito acordado: Home y componentes predefinidos no dependen de Tool para existir; Tool vincula ids y estructura, no genera automáticamente componentes. Distinguir falta de Tool/configuración de falta de conexión y de falta de KPI. El Home debe poder mostrar estados de ausencia/indisponibilidad y, cuando previamente está configurado, recuperar sus datos tras restablecer los workers/lectores. La implementación/render actual y la recuperación visual no han sido validadas con datos reales en Docker.

La evaluación particular de nuevos componentes dinámicos se define al crearlos. No inventar un enlace universal entre fallo de transporte KPI y estados visuales de PI/Dispatch.

## Fuera de foco actual

Master Projection externa tiene prioridad siguiente. Mantener cold-start Web, pruebas Home con Tool/Delivery reales, readiness funcional, credenciales y reglas de bloqueo de ejecución como temas OPEN delimitados, sin introducir cambios de Web durante el cierre documental de Resource Preparation.
