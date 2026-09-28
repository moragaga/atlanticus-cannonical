# Web Platform — Open Items

Estado: **CURRENT — MASTER 001A/001B/001C/001D.1/001D.2 IMPLEMENTADOS; 001D.3 LOCAL NAVIGATION CLOSED; 001D.4 LOCAL DISTRIBUTION SYNC CLOSED**.  
Corte funcional Master `atlanticus@9c6daffd04b9c249f75a55b6cdb9b44e6d92a795`; Docker de Master `ca3ee5084542e393c105b49e98b3c282da56f7fb`. Estado anterior de esta página relativo a “001D sólo diseño” queda **SUPERSEDED**. No recalificar otros frentes desde este documento.

## CLOSED/CURRENT — evidencia estrictamente acotada

- **Master 001A/001B/001C:** generador de material protegido, reader/credencial de servicio, planner sobre seis pares y página HTTP independiente; códigos publicados en sus checkpoints anteriores.
- **Master 001D.1/001D.2:** executor de target exacto, controles del backend, workflow HTTP prepare/confirm/aplicar por operación con revalidación y CSRF; **VERIFIED USER-REPORTED** 50 pruebas seleccionadas PASS + Ruff/diff check PASS. Commit de corte funcional: `ca3ee508`.
- **Master 001D.3:** **CLOSED en alcance Docker local Navigation**: distribución `ca3ee508`, 67 wheels `BUILT_UNQUALIFIED`, `PRECHECK_PASS`, `SYNCED`, material externo/login y prueba UI de modificación/publicación Navigation→Master prepare/confirm→Manager recargado proyectado/sincronizado. No acreditación de los otros cinco ni de producción.
- **Master 001D.4:** **CLOSED en alcance de sincronización local**: correctivo `9c6daffd` instaló Starter como paquete durante `project.py sync`; **VERIFIED USER-REPORTED** 24 tests y Ruff/diff check PASS; worktree limpio, 67 wheels, `BUILT_UNQUALIFIED`, `PRECHECK_PASS`, `SYNCED`, import y CLI Master sin `PYTHONPATH`, segundo `sync=ALREADY_SYNCED`.
- **Users Recovery/Proyección de usuarios del Manager:** cerrado solo para el flujo previamente validado en laboratorio. Master Users REPLACE **no** se implementó.
- **Resource Preparation 001:** cerrado para recursos Docker locales ya reportados; Home cold start sigue OPEN.

## OPEN / BLOCKED / UNVERIFIED — por qué

| Ítem | Estado | Frontera exacta pendiente |
|---|---|---|
| Reconciliación canonical de Master y Starter | **PLANNED — SIGUIENTE FOCO ÚNICO** | Aplicar documentalmente los reemplazos propuestos tras verificar de nuevo canonical/decisions/implementación; ninguna modificación de código en este paso. |
| Nueva distribución `9c6daffd`: Docker image/runtime | **OPEN / UNVERIFIED** | El precheck reportó explícitamente `image_build: UNVERIFIED` y `runtime: UNVERIFIED`; el Docker positivo 001D.3 fue del commit anterior `ca3ee508`. No transferirlo a la nueva imagen. |
| Master Apply de los otros cinco dominios | **OPEN / UNVERIFIED E2E** | Tests del executor existen, pero el recorrido Docker operador→Cosmos→Manager solo se observó en Navigation. |
| Fallos parciales/concurrencia real/auditoría operativa de Master | **OPEN / UNVERIFIED** | La revalidación de target está implementada; no existen aquí logs de fault injection ni certificación de atomicidad, rollback o monitoreo durable Master. |
| 001C logout/relogin manual y seguridad multiworker | **OPEN / UNVERIFIED MANUAL** | Comportamiento y tests del código no equivalen a ensayo browser explícito de sesión/logout ni a host multiworker productivo. |
| Master material: pipeline productivo | **OPEN / UNVERIFIED** | La ruta de ZIP externo está implementada; custodia, provisión, warmup/upload si fuese necesario, rotación y revocación operativa siguen sin contrato/qualification integral. |
| Baseline Entra pre-Manager frente a excepción Master | **OPEN / DOCUMENTAL CONTRACT GATE** | La canonical histórica registra la tensión. No se auditaron exhaustivamente DOCX de identidad en decisions; no atribuir contradicción confirmada sin contrastarlos. |
| Users Master REPLACE y destino sin promovidos | **BLOCKED / OTHER FOCUS** | Resolver `identity_realm`, snapshot explícitamente aprobado, issuer/subject, autorización, mantenimiento y transporte antes de incorporar ejecución externa. |
| Users fallos parciales/revocación/reintento | **OPEN / SECURITY GATE** | UI y pruebas previas del Manager no acreditan aislamiento operacional, revocación real ni recuperación tras interrupción a mitad de REPLACE. |
| Python baseline 3.14.7 vs Web/Starter 3.14.2 | **OPEN / SEPARATE** | No mezclar migración transversal de versión con Master/documentación. |
| Resource Preparation cold start, Home/Collector, Azure/Entra/CI | **OPEN / OTHER FOCI** | Evidencia histórica por checkpoint; no se verificaron como parte de 001D. |

## Pendientes de otros frentes previamente registrados — preservar sin abrirlos

Los siguientes ítems ya figuraban en el documento canónico anterior. Su enumeración **no** abre trabajos durante este cierre ni acredita nueva ejecución:

| Ítem previo | Estado conservado | Motivo |
|---|---|---|
| RESTORE/REPLACE invasivo de Users desde navegador | UNVERIFIED | La aceptación visual/servicio no reemplaza un ensayo destructivo completo en browser. |
| Transporte interambientes de snapshots aprobados | OPEN / OTHER GATE | Verificar compatibilidad de `issuer`, registro y proyección destinataria. |
| `APPLICATION-RESOURCE-PLAN` global | PLANNED / OTHER FOCUS | Resource Preparation 001 solo cubre recursos acordados del Manager ADA. |
| Publicación/proyección E2E de documentos de negocio reales | UNVERIFIED / OTHER FOCUS | Preparar recursos locales y ensayar Navigation no valida la totalidad de documentos de negocio. |
| Home con Cosmos vacío/caído | PLANNED / LATER FOCUS | Persisten criterios de estado ausente/no disponible y carga Tool. |
| Reanudación real del KPI Collector tras caída de Cosmos | UNVERIFIED E2E / LATER FOCUS | Tests unitarios/cache no sustituyen un ensayo completo Home/browser. |
| Estados futuros de componentes dinámicos | PLANNED / LATER FOCUS | Evaluar por componente cuando se implemente, sin atribución especulativa de errores. |
| Identificación organizacional ADA de Users | PLANNED / OTHER FOCUS | Cargo/área/grupo requieren ownership propio fuera de Core Users genérico. |
| Diagnósticos más detallados de proyección ante caída Cosmos/Blob | PLANNED / OTHER FOCUS | Política operacional/UI diferida; no ampliar Master por conveniencia. |
| Ruff global, CI monorepo y telemetría externa | UNVERIFIED / SEPARATE | Ninguna de las baterías acotadas acredita integridad global. |

## Invariantes y exclusiones

`Master != Manager`; `planner.read_only != execution permission`; `ProjectionTarget` exacto con dependencias; revalidación servidor y confirmación humana; `Users REPLACE != séptimo Source/Projection`; Blob de registro con candidatos != snapshot aprobado. Sin nuevo proceso coordinador, variables para reemplazar rutas, adaptadores legacy, cambios indirectos sobre alarmas/KPI ni reetiquetar PRECHECK como qualification Docker final.

**Siguiente paso único:** consolidar estas actualizaciones en `atlanticus-cannonical` tras revisar los tres HEAD y las decisiones pertinentes. No comenzar implementaciones desde esta lista durante el cierre.
