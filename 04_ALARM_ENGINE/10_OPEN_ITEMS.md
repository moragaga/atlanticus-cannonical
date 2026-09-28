# Alarm Engine — Open Items

Estado: **B2c.7a/b/c/d CLOSED en implementación y gates locales reportados; distribución/Docker PLANNED; histórico v1 condicionado**. Corte 2026-09-28. Commit del hito verificado en remoto `atlanticus@c67fcb5b105cc561c16719a8bca4ea5aa74c3fae`, `main@bc1d73742bcb04eb495bbbb1725a8ad23d4eff38` avanzó únicamente en ADA Generic; decisions `50c2bb3...`, canonical base `5558cf9...`. Las pruebas compartidas NO son CI de checkout limpio.

## CURRENT / CLOSED según evidencia delimitada

- Source v3; READY/BLOCKED local exacto; B1 exact pin; B2a WAL adopción global V1/V2; B2b EFFECTIVE recuperable; B2c runtime y sesión fijada. Estos contratos preexistentes permanecen congelados.
- B2c.7a Engine publica `current/latest.json` v1 completo y FACTS durables, con cursor del productor; los schemas fuente son estáticos, no datos runtime.
- B2c.7b job Delivery independiente consume publicación Engine y materialización exacta, persiste copia de CURRENT/FACTS y su cursor independiente; no usa el WAL como feed.
- B2c.7c integración local controlada real Engine→Delivery y reinicios simulados por recreación de componentes (1 específica PASS y 152 PASS/1 SKIPPED conjuntos en su gate).
- B2c.7d reemplaza FACTS runtime v1 por v2 encadenado; tests locales de huecos, manipulación/reordenación, interrupciones y recovery (32 específicas, 162 PASS/1 SKIPPED conjuntos y Ruff PASS).

## OPEN y razón, sin crear nuevos frentes durante este cierre

| Elemento | Estado | Razón / tratamiento autorizado después |
|---|---|---|
| **Distribución/artefactos Engine + Delivery** | **PLANNED — siguiente foco único** | No hay gate final de wheel/distribución del commit B2c.7d ni inspección de contenido de contratos y dependencias del paquete distribuido. Auditar artefactos actuales antes de implementar algo. |
| **Docker Engine + Delivery independiente** | **PLANNED — mismo foco único** | B2c.7c usó volumen compartido en prueba controlada e instancias reiniciadas, no contenedores/procesos físicamente separados. Verificar montaje y recovery reales. |
| **Volúmenes/cursores FACTS v1 preexistentes** | **BLOCKED si se intenta desplegar v2 sobre histórico** | Productor/receptor v2 rechazan estado v1 sin decisión controlada. Inventariar ambiente; no borrar historia ni agregar adaptador temporal o migración presunta. Puede diferirse si el objetivo documentado es entorno nuevo sin histórico. |
| `1 skipped` en suite combinada | **UNVERIFIED** | El log no identifica su nombre/motivo. Determinarlo durante próximo gate, sin presumir prueba distribuida aprobada. |
| CI limpio del HEAD `c67fcb5...` | **UNVERIFIED** | El commit y su diff ya son verificables en Git, pero los logs se ejecutaron sobre árbol local antes del commit: no hubo CI ni checkout limpio de ese SHA. |
| Volumen multi-host/semántica real y lease/fencing entre procesos | **UNVERIFIED** | Tests usan `tmp_path` y controlled fencing; faltan ensayos físicos. |
| Prueba con fuentes reales / Tool GREEN / evaluator qualification | **UNVERIFIED / SEPARATE** | Ejemplo NOTPII no prueba datasets, sistemas Azure ni catálogo productivo real. |
| Full Live Delivery/`AlarmLiveProjection` | **PLANNED / SEPARATE** | Input receiver es staging, no aplica regla visual de publicación, causa ni consume Delivery Configuration para construir Live result. |
| Management Capture, publicaciones/escalamientos reales de Delivery | **PLANNED / SEPARATE** | No existen hechos propios de dispatch/escalation ni round-trip Web en este hito. |
| History/Analytics read model | **PLANNED / SEPARATE** | FACTS durable transporta hechos; no hay read model consultable ni política aprobada de retención. |
| Cambios/remociones de keys y migración amplia de alarmas | **OPEN / diferido expresamente** | El usuario excluyó esta frontera de B2c.7; no introducir efectos implícitos. |
| Semántica y UI de desactivación «fin del turno» | **OPEN / SEPARATE** | Requiere contrato Web/Domain/calendario; no inventar enum o cálculo. |
| Adoption B.1 contra implementación (`evaluator_key`/`kind`/`priority_group`) | **OPEN / CONFLICT** | Intención de decisiones y rechazo observado del ejecutor no están reconciliados. |
| B.1 Special Cascade, Messages inactivos y visual target/routing | **OPEN / CONFLICT documental/semántico** | Ver `09_DECISION_INDEX.md`; no resolver cambios fuera del incremento. |
| Python 3.14.7 global vs Command Center 3.14.2 | **OPEN / SEPARATE** | No alterar metadata incidentalmente durante gate de distribución sin decisión explícita. |
| Cosmo/Blob/Azure reales, topologías productivas, costo/performance | **UNVERIFIED / SEPARATE** | No acreditado por gate unitario/integración local de este chat. |

## Contratos que no se reabren al entrar al siguiente foco

```text
B.2 READY != EFFECTIVE; pin source/result/hash/Rn-Cn exacto
Engine WAL único + durable/materialized + EFFECTIVE derivado
Engine CURRENT v1, snapshot completo y vacío válido
Engine FACTS runtime v2 estricto, batches inmutables y previous_batch
Engine export cursor independiente de Delivery consumption cursor
Delivery sólo consume output Engine y artifact exacto, no WAL
No v1 runtime adapters, no data reset, no API Web/Analytics nueva
```

**Siguiente frontera única recomendada:** verificar primero distribución de artefactos existentes y ejecutar Engine/Delivery en Docker independiente. Si se detecta histórico v1, documentar BLOCKED y solicitar decisión; no realizar migración incidental. No abrir Live/History mientras se ejecuta este gate.
