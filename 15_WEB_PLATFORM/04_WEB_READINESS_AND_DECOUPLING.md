# Web Platform — Readiness and Decoupling

Estado: **CURRENT CONTRATO / RESOURCE PREPARATION LOCAL VALIDATED / HOME DEGRADATION PARTIAL AND OPEN**  
Implementación inspeccionada: `atlanticus@da75752e87036b8318f38f8d405c55e8cb18717d`. Pruebas Docker reportadas el 2026-09-28; no sustituyen pruebas de UI ni Azure.

## Invariante

El proceso Web, la capacidad de configuración, los proveedores externos y los datos de negocio tienen disponibilidades independientes:

```text
WEB PROCESS / BASE COMPOSITION != CAPABILITY READY
APPLICATION EXISTENCE != TOOL CONFIGURATION EXISTENCE
TOOL CONFIGURATION EXISTENCE != KPI DELIVERY DATA EXISTENCE
```

La Web base no debe dejar de existir por ausencia de datos ni por un proveedor temporalmente indisponible. La indisponibilidad de repositorios de autorización **nunca** concede permisos de Manager.

## Tool Projection — CURRENT

| Estado | Semántica |
|---|---|
| `READY` | Projection válida/utilizable. |
| `UNCONFIGURED` | Provider accesible, sin Tool vigente. |
| `UNAVAILABLE` | Provider inaccesible. |
| `INVALID` | Contrato/documento inválido. |

La composición real utiliza `AdaGenericSettings` y Tool Persistence; resuelve Tool Projection en el arranque. `READY` habilita el binding operativo y, con configuración KPI Delivery completa, conecta el collector. Los otros estados permiten crear una definición Web base y generan diagnósticos. No reproyecta desde Source al atender una lectura ni cambia silenciosamente de provider. `ToolStructure` entrega ids/vinculaciones, **no crea automáticamente los componentes visuales de cada Tool**.

**Limitación CURRENT:** la resolución inicial de Tool es síncrona. Si Cosmos está inaccesible desde el primer instante, esa resolución puede retrasar la inicialización de los workers. Si arranca sin Tool, no adjunta posteriormente el collector por el mero hecho de recuperar Cosmos. Esa recomposición tardía no se ha implementado ni probado.

## KPI Collector — CURRENT

Se crea por Tool válida/configuración Delivery; una instancia de cache/poller pertenece a cada worker, y la primera solicitud operacional elegible inicia su thread. `health`, `assets` y rutas de autenticación no inician polling. Defaults: Latest 10 s, Timeseries 120 s, refresco browser 10 s. Browser lee cache del proceso, no Cosmos inline.

- Primer documento inexistente: `MISSING`; los Component Stores iniciales continúan vacíos.
- Documento desaparecido después de uno válido: conserva último snapshot bueno y devuelve `MISSING`.
- Documento incompatible o con antigüedad regresiva: no reemplaza un snapshot vigente válido.
- Error Cosmos de lectura: `KpiDeliveryReadError` se registra en el polling, preserva la cache y se continúa intentando; recuperación con datos reales tras interrupción **UNVERIFIED end-to-end**.
- Un dato conservado en cache no es automáticamente fresco. El código inspeccionado de Content State/Time Status transforma la frescura de PI/Dispatch en `STALE`/`SOURCE_ERROR`, pero **no demuestra** una conexión automática entre error del transporte Cosmos KPI y `SOURCE_ERROR` visual para todos los componentes.

No cambiar polling ni coherencia por conveniencia de esta frontera; diseñar evaluación detallada al incorporar nuevos componentes.

## Qualification de Web durante fallos — VERIFIED USER-REPORTED CON LÍMITES

1. En `ada-generic:resource-validation-001`, Web arrancó con emuladores nuevos y respondió `/health/live` HTTP 200 mientras el job `resources` seguía inicializando.
2. Con Web previamente iniciada, detener Cosmos, validar fallos y restablecerlo no detuvo Gunicorn. Se observó HTTP 200 durante el fallo y, tras recuperación, ocho recursos `READY` sin reiniciar Web.
3. Al **recrear Web con Cosmos ya detenido**, Gunicorn inició y anunció tres workers, pero `curl /health/live` agotó diez segundos sin respuesta. Después de restablecer Cosmos, el mismo contenedor se observó `healthy`, `/health/live` devolvió 200 y `/health/ready` 200 con `checks: {}`. No afirmar que el Home fue accesible durante la indisponibilidad inicial.
4. `checks: {}` significa que no existían checks funcionales registrados en esa ejecución; el estado `ready`/200 **no verifica Cosmos ni Tool Projection**.

Ninguna de esas comprobaciones equivale a una sesión de navegador con Tool válida, documento KPI real y caída/recuperación de delivery.

## Contrato del Home — DECIDED, no confundible con código ya probado

La estructura predefinida del Home pertenece a la aplicación y debe permanecer visible aunque Cosmos esté vacío o temporalmente indisponible. Tool proporciona ids/contratos y vinculaciones; no se le atribuye creación automática de visualizaciones. Distinguir **sin Tool configurada** de **Tool temporalmente inaccesible**. Los componentes conocidos deben poder representar ausencia/no disponibilidad sin inventar valores. Si existen datos buenos anteriores, conservarlos puede ser correcto, pero deben distinguirse de datos vigentes mediante un estado de frescura/error sustentado por evidencia.

**CURRENT STATIC / OPEN FUNCTIONAL:** el layout actual incluye elementos condicionales (por ejemplo Global Indicators y cuerpo vinculado) y no está probado que exhiba *todos* los componentes conocidos con error cuando falta Cosmos/Tool. No confundir la supervivencia HTTP con continuidad visual/funcional del Home. La recuperación de una Tool nunca cargada es diferente de los reintentos de un collector ya conectado.

**PLANNED / FRENTE POSTERIOR:** ensayar y, si hace falta, ajustar el Home predefinido y recuperación de datos en browser sin reinicio cuando hay Tool y delivery reales. Para componentes dinámicos futuros, definir la evaluación específica `MISSING`, retraso y fallo de fuente durante su propio incremento.

## Manager / identidad CURRENT

`ADA_MANAGER_PERSISTENCE_PROVIDER` admite `auto`, `local`, `durable`, `disabled`; `durable` es modo de persistencia, no retención. El CLI durable local usa Blob/Cosmos; en producción se requiere `IdentityProvider` productivo inyectado desde host. Conexiones Tool Source y Tool Projection conservan ejes independientes salvo los requisitos concretos del Manager durable.

**Finding estático histórico OPEN:** `bootstrap._prepare_manager_identity()` crea un `AccessRuntime` mientras `create_identity_module()` puede registrar otro. Impacto de sincronización bajo solicitudes reales no está verificado; no introducir un adaptador ni modificarlo durante este cierre.

## Fronteras

- El ensayo de aprovisionamiento local **CLOSED** no certifica Home degradado ni recuperación de un collector con datos reales.
- La carga diferida de Tool, la configuración de readiness real y los permisos que bloquean proyecciones durante fallos se mantienen **OPEN**, sin ampliar el siguiente incremento Master.
- Backend workers, proceso Web y browser son superficies distintas; demostrar liveness no prueba callbacks funcionales ni datos entregados.
