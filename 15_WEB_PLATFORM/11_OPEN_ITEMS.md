# Web Platform — Open Items

Estado: **CURRENT / RESOURCE PREPARATION 001 LOCAL SCOPE CLOSED / MASTER PROJECTION NEXT**  
Implementación contrastada: `atlanticus:main@da75752e87036b8318f38f8d405c55e8cb18717d`; decisions `@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`. Qualification de este cierre: logs y pruebas Docker locales del usuario, no CI/Cloud.

## CLOSED en sus alcances validados

- **Users Recovery backend y Users Projection del Manager**: snapshots aprobados, RESTORE estricto y REPLACE, before-image/auditoría; UI con pestañas y modal. Son los cierres previos descritos en `13_USERS_PROJECTION_RECOVERY.md`. No confundir con Master externa ni con una calificación integral de producción.
- **Resource Preparation 001 de ADA Generic**: código actual incorporado en `da75752`, espejos pedagógicos y pruebas. En Docker local se verificaron la preparación de ocho recursos desde cero, reejecución sobre recursos existentes, reinicio de emuladores conservando estructura, fallo parcial de Cosmos y recuperación de la validación. La Web previamente iniciada permaneció viva durante la indisponibilidad. Ver `03_RESOURCE_PROVISIONING.md`.
- **Cierres históricos de Starter, Navigation y Manager**: respetar su evidencia por versión; no atribuir todos los tests o compatibilidad productiva al HEAD actual.

## OPEN / razón / frontera

| Ítem | Estado | Razón y alcance pendiente |
|---|---|---|
| `MASTER-PROJECTION-001` página externa | **PLANNED / NEXT ÚNICO** | Definir material de acceso y wiring con tooling/warmup actual; después planificar proyecciones aplicables reutilizando contratos existentes. |
| Material Master ausente | PLANNED / ACCEPTANCE | Página informativa controlada, sin autenticación ni acciones. |
| Material Master íntegro y presente | PLANNED / ACCEPTANCE | Autenticación de servicio, permisos de operación, estado/plan, confirmación, trazabilidad y reintentos. |
| Excepción pre-Manager vs baseline Entra anterior | OPEN / CONTRACT CONFLICT | Delimitar acceso aislado sin debilitar identidad normal, validar sólo tras acuerdo. |
| Users Master en destino sin promovidos | OPEN / DESIGN GATE | El provider del Manager deriva `identity_realm` de promovidos; no reutilizarlo automáticamente en destino vacío. |
| Fallos parciales y revocación de Users REPLACE | OPEN / SECURITY GATE | Faltan interrupción real, retries, aislamiento/concurrencia y revocación efectiva. |
| Users RESTORE/REPLACE invasivo desde UI | UNVERIFIED | Prueba positiva desde CLI Docker y aceptación visual no prueban operación invasiva en navegador. |
| Transporte interambientes del snapshot aprobado | OPEN | Compatibilidad real de `issuer/subject_id`, destino Cosmos y registro autorizado deben ser explícitos. |
| `APPLICATION-RESOURCE-PLAN` global | PLANNED / OTHER FOCUS | Incremento 001 cubre seis Cosmos del Manager y Blob; no cubre todo ADA ni Atlanticus. |
| Preparación/telemetría Azure productiva | UNVERIFIED / OTHER FOCUS | Falta infraestructura real, permisos, acceso, observability remota y ensayos. |
| Persistencia de documentos reales y pipeline proyección end-to-end | UNVERIFIED / OTHER FOCUS | Reiniciar emuladores conservó topología, no acreditó documentos de negocio ni su publicación. |
| Arranque frío de Web con Cosmos detenido | OPEN / DEFERRED | Se observó timeout HTTP de 10 s; después de recuperar Cosmos, Web respondió. No ocultar ese resultado ni declarar estabilidad Home. |
| Home predefinido con Cosmos vacío/indisponible | PLANNED / LATER FOCUS | Acuerdo: estructura visible, Tool no crea visualizaciones; ausencia vs indisponibilidad son estados distintos. No está calificado visualmente. |
| Reanudación real de KPI Collector tras caída Cosmos | UNVERIFIED E2E / LATER FOCUS | Reintentos y conservación de cache existen en código/tests unitarios; falta Tool y delivery reales con browser en Docker. |
| Evaluación visual para futuros componentes dinámicos | PLANNED / LATER FOCUS | Definir estados concretos al crearlos, sin imputar automáticamente `SOURCE_ERROR` de PI/Dispatch a errores de KPI Delivery. |
| Identificación organizacional ADA por usuario | PLANNED / OTHER FOCUS | Cargo, área operativa y grupo; Source/Projection y ownership no definidos aquí. No añadir a Users genérico. |
| Política de bloqueo para ejecución/proyección con Cosmos o Blob caídos | PLANNED / OTHER FOCUS | Usuario decidió diferir detalle de UI y workflow; no implementarlo en Resource Preparation. |
| Python `3.14.7` objetivo vs metadata Web `3.14.2` | OPEN / SEPARATE | Confirmado en `pyproject.toml` y tooling, fuera de Master. |
| Full CI/Ruff definitivo sobre checkout limpio `da75752`, Entra Azure y pruebas generales | UNVERIFIED / SEPARATE | Las pruebas reportadas se ejecutaron antes de publicar el commit. |

La hipótesis histórica `extra` Users/Cosmos sigue siendo idea diferida, sin autorización para incorporarla. Los registros históricos y documentos de alarmas pertenecen a frentes separados.

## Orden aprobado para continuidad, sin mezclar incrementos

1. **NEXT ÚNICO:** `MASTER-PROJECTION-001`: lectura actual de implementación/decisions/canonical, diseño de contrato y posterior implementación incremental autorizada.
2. Después, en incremento independiente: vinculación de identidad ADA con cargo, área operativa, grupo y origen/autorización de datos por definir.
3. Después, en incremento independiente: estabilidad/continuidad del Home y recuperación observable de collectors con datos reales.
4. La evaluación detallada de nuevos componentes corresponde a su propio incremento de creación dinámica.

Esta secuencia no convierte el trabajo diferido en decisiones técnicas implementadas ni bloquea el debate Master. El requisito de material Master ausente/presente ya figura en `06_PRE_MANAGER_BOOTSTRAP_SURFACE.md`; no modificar esos contratos mientras no aparezca una contradicción real.
