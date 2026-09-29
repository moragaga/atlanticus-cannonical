# ADA Command Center — Golden Path

Estado: **PARTIALLY IMPLEMENTED — Materialization/Engine/Delivery input B2c.7 validados localmente; Live/Web/History y Docker de distribución PLANNED**. Corte 2026-09-28. Commit del hito `atlanticus@c67fcb5b105cc561c16719a8bca4ea5aa74c3fae` verificado en Git; main `bc1d73742bcb04eb495bbbb1725a8ad23d4eff38` tiene cambio posterior ajeno al alcance. No revalidar otros frentes por inferencia.

## Recorrido objetivo con estados delimitados

| Paso | Owner | Estado de este corte |
|---|---|---|
| Tool owners publican proyecciones; Manager reconcilia Cn | Tool/Manager | CURRENT preexistente; no revalidado en B2c.7. |
| Confirmed Tool Catalog Cn → Storage durable objetivo | Tool | CURRENT preexistente; Azure físico UNVERIFIED. |
| Alarm authoring pin Cn, Validate/Publish y drift guard | Alarm Manager | CURRENT preexistente. |
| Source release Rn congela `ToolDependencyManifest(Cn)` schema v3 | Alarm Configuration | CURRENT. |
| Persistencia/proyección operativa de entrada | Web/Materialization | CURRENT en código; infraestructura Cosmos física UNVERIFIED. |
| B.2 adquiere proyección, aplica qualification y resuelve | Materialization | CURRENT; qualification real Green UNVERIFIED. |
| Materialization READY local con manifest+Runtime/Delivery o BLOCKED | Materialization | CURRENT; salida Cosmos monolítica antigua SUPERSEDED. |
| B1 exact pin; B2a WAL adoption V1/V2; B2b EFFECTIVE derivado | Engine/Persistence | CURRENT; recovery local validado en generaciones anteriores. |
| Runtime ejecuta sesión efectiva y publica CURRENT completo v1 | Engine | CURRENT, B2c.7a tests locales PASS. |
| Runtime exporta sólo commits durables como FACTS v2 encadenados | Engine | CURRENT, B2c.7d tests locales PASS. |
| Job separado recibe CURRENT/FACTS con pin exacto y cursor propio | Delivery input | CURRENT, B2c.7b y d tests locales PASS. |
| Integración Engine→Delivery en volumen controlado/recreación de instancias | Test integrado | CLOSED local B2c.7c; no equivale Docker independiente. |
| Build distribuido + Engine/Delivery como procesos Docker separados | Qualification | PLANNED, foco único siguiente. |
| Enriquecimiento, causa, filtro visibility/priority, AlarmLiveProjection | Live Delivery | CONTRACT AGREED / NOT IMPLEMENTED. |
| Management Capture/Projection y Web operacional | Servicios/Web | PLANNED / SEPARATE. |
| History/Analytics durable consultable | Analytics | CANDIDATE / PLANNED. |

## Qualification B1d — recorrido Tool cerrado sólo en el entorno controlado

**VERIFIED según ejecución local del usuario:** `prepare --apply` y `prepare` validaron dos bases Cosmos con contenedor `ada-tool-projection`; Source→Projection de Process/Integrated Operations pasó; `inspect-catalog` devolvió dos conexiones `READY`, dos candidatos y `can_confirm=true`; la confirmación manual seguida de `verify-catalog` corroboró el blob `conciencia_situacional/command-center/tool-catalog/current.json` con los releases exactos. La UI mostró ambas herramientas y existe una Alarm Source `local` en filesystem.

**UNVERIFIED:** continuidad hasta Alarm Source Blob/Cosmos Projection durable: `verify-alarm` no encontró Source/Projection durable. Este resultado **no** bloquea considerar cerrado el alcance local de qualification de Tool Catalog; tampoco permite llamar completo al Golden Path de alarmas. La futura adopción visual/operacional se validará con una ejecución real, fuera de B1d.

**DECIDED/PLANNED, condición de cierre arquitectónico Web:** extraer Tool Catalog Web independiente, integrar desde host actual y componer/distribuir el Starter `ada-command-center-generic`; limpiar restos temporales después de inventario. Este trabajo es independiente de la qualification Engine/Delivery Docker B2c.7.

## Separaciones obligatorias

```text
Rn/Cn y manifest Tool son exactos al publicar release Source v3.
VALID_AT_SAVE != READY != EFFECTIVE.
READY -> Runtime y Delivery del mismo AlarmResolutionKey y exact artifact pin.
BLOCKED conserva diagnóstico sin artefactos ejecutables.
INVALID != REMOVED; DISABLED != REMOVED; TRACE_ONLY != REMOVED.
WAL -> DURABLE -> MATERIALIZED -> EFFECTIVE projection.
Engine CURRENT v1 (snapshot) != FACTS v2 (hechos encadenados).
Engine export cursor != Delivery consumption cursor.
Delivery input receiver != AlarmLiveProjection.
```

Una confirmación Tool C2 posterior no cambia retroactivamente `R1/C1`. Engine y Delivery no necesitan releer su configuración desde Cosmos: usan versión exacta local tras EFFECTIVE; el mecanismo físico final para Live Web sigue abierto.

## Próximo entregable único y límites

Validar distribución existente de ambos jobs (dependencias, entrypoints, schemas y empaquetado) y ejecutarlos separadamente en Docker con volumen compartido y reinicios. La prueba omitida sigue sin identificar; el estado v1 real preexistente puede bloquear despliegue v2 y requiere inventario/decisión sin legacy ni borrado. No abrir en paralelo Live Projection, Web, Management, History ni nuevos productores de qualification.
