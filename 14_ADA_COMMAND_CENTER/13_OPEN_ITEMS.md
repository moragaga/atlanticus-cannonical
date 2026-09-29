# ADA Command Center — Open Items

Estado: **CURRENT — C1 Web Tool ownership CLOSED; C2 identidad/Source Key/topología Cosmos CLOSED estructuralmente y con gates locales delimitados. C3/C4/C5 PLANNED; Starter, durable E2E, Docker, Live/Management/Analytics y UX pendientes independientes**.

## Autoridad del corte

```text
atlanticus:main           18029e19ff01e58b9c9399c132ff32b5ca913f06
atlanticus-decisions:main 50c2bb3f7bf21b05444a102d4502250a5c8a7d2e
canonical:main previo    15ba51fb5a140601fd4e2a8d78a01a5b87c6eeaa
```

El commit remoto actual incluye la corrección de una prueba operacional realizada por el usuario; no modificarla ni incorporar tests extra durante este cierre documental.

## Estado consolidado por elemento

| Elemento | Estado | Evidencia / frontera |
|---|---|---|
| C1 Catalog/Discovery/Tool UI propiedad Web | **CLOSED / CURRENT** | Bibliotecas bajo `web/tools` y composición host; `backend/tools` SUPERSEDED; qualification B1d local histórica. |
| C2 APPLICATION operacional común | **CLOSED / CURRENT** | Tres plantillas/manifiestos declaran `ada-command-center`; `job_key` separa sus leases. |
| C2 VOLUMEN_PATH manual y compartido | **CLOSED contractual / UNVERIFIED físico** | Tres procesos conservan configuración absoluta manual; no hay prueba multi-host del mismo montaje. |
| C2 Source Key única | **CLOSED / CURRENT** | Texto `alarm-configuration` en `domain/alarms`, Web y procesos consumidores. Variables env/secret redundantes eliminadas. |
| C2 Alarm Cosmos physical name/partition | **CLOSED en código / UNVERIFIED físico** | Materialization y Web consumen resource contract existente; misma cuenta/base real no acreditada. |
| Backend completo después del commit final | **OPEN / UNVERIFIED** | El último gate reportado fue 238 PASS/1 SKIP/1 DESELECT antes del arreglo; usuario corrigió el test en Git, falta log sin exclusión. |
| Qualification Materialization C3 | **PLANNED / BLOCKED BY DESIGN** | JSON manual CURRENT, sin productor/validadores GREEN productivos verificados. |
| Delivery CURRENT-only C4 | **PLANNED** | Receiver actual continúa CURRENT+FACTS/cursor; objetivo de siguiente debate. |
| Evidencia técnica/environments C5 | **PLANNED / OPEN** | `ALARM_TECHNICAL_EVIDENCE_CONTRACT_KEY/VERSION` necesitan owner y valores contractuales reales. |
| Docker separado + volúmenes reales | **UNVERIFIED / SEPARATE** | Integración local controlada histórica no equivale a distribución aislada. |
| Alarm Source Blob / Projection Cosmos E2E | **UNVERIFIED / SEPARATE** | Contratos/adapters presentes; binding real y publicación/proyección física sin gate reportado. |
| Starter Web propio | **PLANNED** | Host temporal CURRENT, browser/aceptación final UNVERIFIED. |
| Live Delivery / AlarmLiveProjection | **CONTRACT AGREED / PLANNED** | Input receiver no es Live materializer; no mezclar C4 con Live. |
| Management Capture/Projection, History/Analytics | **PLANNED / SEPARATE** | No inferir de WAL/FACTS disponibles ni leer WAL desde Web. |
| UI desactivación fin de turno y guardado modal | **OPEN / SEPARATE** | Requiere contrato temporal/calendario y validación visual/funcional explícita. |
| `backend/materialization -> web/projection-cosmos` | **OPEN / SEPARATE** | Importación actual conocida; C2 no reubicó la dependencia. |

## Contratos CURRENT preservados

- Source `AlarmConfigurationSnapshot` v3 congela ToolDependencyManifest Rn/Cn; Save/Validate/Publish no reinterpreta Tool latest.
- C2 fija la Source Key textual única `alarm-configuration` en Domain, no en cuatro configuraciones editables.
- C2 comparte valor de APPLICATION sin compartir `job_key`; `VOLUMEN_PATH` sigue siendo input manual y debe apuntar físicamente al mismo almacenamiento.
- Cosmos Alarm Projection tiene nombre físico/partición declarados en su resource contract, solo connection ref admite override; nombre Blob sigue configurado ambientalmente.
- `VALID_AT_SAVE != READY != EFFECTIVE`. READY publica Runtime y Delivery conjuntamente, BLOCKED no sustituye READY íntegro.
- Pin exacto `source_key + result_id + manifest_sha256 + resolution_key`; Engine WAL/EFFECTIVE es autoridad operacional.
- Engine CURRENT v1 completo y FACTS v2 encadenados son productos distintos. Delivery **hoy** recibe ambos y conserva cursor de FACTS.
- Live Projection, Management Projection y History/Analytics son fronteras distintas; Web no calcula priority/routing ni lee WAL.

## C3, C4 y C5 — estados estrictamente separados

**C3 — PLANNED / BLOCKED BY DESIGN.** Antes de automatizar `ALARM_QUALIFICATIONS_FILE` identificar fuente/produtor GREEN, evaluadores y verificación reales. El input manual existente es una operación legítima controlada; el sistema verifica coherencia antes de promover READY.

**C4 — PLANNED / próximo foco propuesto.** Debatir cambio de `LocalAlarmDeliveryReceiver` y composición del job para consumir el último CURRENT sin backlog FACTS, manteniendo chequeos exact pin y EFFECTIVE. Auditar semántica de reinicios, timestamps y rutas existentes; **no** eliminar productor FACTS v2, WAL, componentes History ni implementar Live por inferencia. Revisar tests afectadas antes de implementar.

**C5 — PLANNED.** Auditar valores reales y propiedad de contrato técnico de evidence Runtime; no inventar constantes ni alterar producción para limpiar `.env` cosméticamente. C2 únicamente quitó variables duplicadas demostrablemente contractuales.

## Conflictos y diferidos

1. **Canonical previo frente a implementación C2:** documentos `00`, `02`, `04`, `11`, `12`, `13`, `17` seguían diciendo C2 PLANNED o anunciándolo como próximo; este paquete los sustituye. Se actualizan también `03` y `06` por consumo Web y estado operacional C2. La base canónica remota permanece antigua hasta integración humana.
2. **Decisions históricos SharePoint/Tool Cosmos frente a pipeline actual:** no resolver silenciosamente; ver `12_SOURCE_LEDGER.md`. La separación Live/Management sigue compatible.
3. **Fuente registral Operational Data frente a test previo de Runtime:** Git actual ya corrige el test por el usuario; no tratarlo como nuevo incremento ni aportar otro parche. La habilitación real de rutas de `FABRICA_KPIS`/`METEODATA` en Alarm Runtime es una decisión distinta, no consecuencia del listado global de fuentes.
4. **Python metadata:** paquetes Command Center con `requires-python==3.14.2` frente al baseline 3.14.7 declarado y entorno informado por usuario; revisión distribución separada, no modificar dependencia por inferencia.
5. **Otros conflictos** documentados en `16_ALARM_LIVE_DELIVERY_CONTRACT.md` (supresión Special Cascade y política de Messages) permanecen OPEN, sin intervención C2.

## OPEN heredados de Web/Authoring que no se abrieron en C2

- **Fin del turno:** hoy `default_deactivation.max_duration_hours` usa campo numérico y Domain requiere un entero `1..12` si la capacidad se habilita. `effective_until` UTC no define por sí solo `shift_end`. Antes de editar decidir zona, calendario/turno Mine o Plant, aprobación y overrides Message.
- **Modal:** UX debe cerrar únicamente tras guardado exitoso; validación visual y funcional independiente.
- **Routing visual:** la separación conceptual visual-target/routing no debe darse por implementada en el editor, que actualmente puede sincronizar targets desde routing. Requiere decisión de experiencia propia.
- **Revision Tool Cn:** aviso UX de cambios respecto al pin del workspace sigue OPEN.
- **Management:** lifecycle de requests deactivation obsoletas y otros conflictos de Core/decisions no se reabrieron aquí.

## Próximo foco único recomendado

**C4: frontera de recepción Delivery CURRENT-only**, en chat separado y con auditoría/diseño antes de autorización de código. Git SOLO LECTURA por defecto. No abrir C3/C5/Live/History/Starter/Docker en ese mismo incremento.
