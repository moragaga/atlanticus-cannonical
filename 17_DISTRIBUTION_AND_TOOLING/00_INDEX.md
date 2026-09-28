# Distribution and Tooling — Index

Estado: **CURRENT — RESOURCE PREPARATION 001 LOCAL CLOSED; MASTER 001A–001D.2 IMPLEMENTADO; 001D.3 NAVIGATION DOCKER LOCAL CLOSED; 001D.4 SYNC LOCAL CLOSED**.  
Master/distribución nueva contrastados en `atlanticus@9c6daffd04b9c249f75a55b6cdb9b44e6d92a795`; Docker Master 001D.3 en `ca3ee5084542e393c105b49e98b3c282da56f7fb`. Canonical base `0f2fff3ec0e71903b5703e03dd6050765d9722ff`. Las pruebas se atribuyen a sus propias versiones, no al HEAD más nuevo por proximidad.

| Archivo | Alcance | Estado |
|---|---|---|
| `01_BACKEND_GENERATION.md` | Artifacts de procesos backend | CURRENT / OTHER FOCUS |
| `02_FRONTEND_GENERATION.md` | Starters y wheelhouses Web, historial de qualifications | CURRENT / HISTORICAL |
| `03_ARTIFACT_DISTRIBUTION.md` | Starter ADA, Resource Preparation, Master 001A–001D.4, evidencias Docker/local sync y gates | **CURRENT — actualizado en este cierre propuesto** |
| `04_SCRIPTS_VALIDATION.md` | Gates de integridad por frontera | CURRENT DIRECTION |
| `05_SUPPORT_SERVICES.md` | Cosmos/Storage y entornos local/productivo | CURRENT DIRECTION |
| `06_ENV_DETAIL.md` | Documentación de variables sin secretos y **única ruta opcional Master** | CURRENT DIRECTION; sin variables nuevas por 001D.4 |
| `07_READMES.md` | README al cierre integral, no durante incrementos parciales | CURRENT POLICY |
| `08_LOADERS.md` | Contratos generales; warmup/upload productivo Master no cualificado | OPEN GATE |
| `09_SOURCE_LEDGER.md` | Historial versionado de verificaciones de tooling | HISTORICAL; no certifica nuevas imágenes |

## Implementación vigente Master distribuido

```text
tooling/distribution/web/generate_starter.py
tooling/distribution/web/ada/{build_distribution,qualify_distribution}.py
tooling/distribution/web/starter/ada/tooling/{project,master_projection}.py
tooling/distribution/web/starter/ada/src/application/master_projection/{material,reader}.py
tooling/distribution/web/starter/ada/src/application/runtime.py
scopes/ada/web/application/ada-generic-application/src/ada/web/application/generic/master_projection/{plan,composition,apply,web}.py
```

`tooling/master_projection.py` existe **dentro del Starter generado**, no en la raíz de `atlanticus`. Se invoca con Python y solicita contraseña interactivamente; genera el material fuera de la distribución, nunca lo empaqueta ni publica en Git. `ADA_MASTER_PROJECTION_MATERIAL_PATH` sigue siendo la ruta **opcional, absoluta y externa**; su uso no demuestra un uploader/warmup productivo.

## Cortes de qualification sin extrapolación

- **Resource Preparation 001 histórico:** ocho recursos locales preparados/READY, repetición idempotente y recuperación desde fallo parcial según logs de usuario. Home cold start no quedó cerrado.
- **Master 001D.3 Docker (`ca3ee508`):** distribución 67 wheels + precheck + sync; material externo y login reales; prepare/confirm de Navigation y Manager recargado proyectado/sincronizado. Cinco dominios pendientes de E2E.
- **Master 001D.4 distribución limpia (`9c6daffd`):** tres archivos corregidos (tooling productivo, espejo, tests); 24 pruebas PASS, Ruff/diff PASS; 67 wheels, `BUILT_UNQUALIFIED`, `PRECHECK_PASS`, `SYNCED`, import y ayuda del generador Master **sin `PYTHONPATH`**, segundo `sync=ALREADY_SYNCED`. No hay nueva imagen Docker calificada para este SHA: `image_build/runtime: UNVERIFIED`.
- **Histórico Starter** Generic 36/ADA 108 ruedas en versiones anteriores: conservar en los documentos históricos sin transferir resultado al build actual.

## Contratos y continuidad

Master no es Manager; preview del plan es read-only, Apply individual requiere acción `projection.apply` y confirmación con target exacto revalidado. Users REPLACE desde Master permanece no ejecutable/bloqueado. No ampliar la excepción exacta de rutas de Master a un prefijo completo. No crear nueva variable, adaptación temporal, proceso de coordinación ni una segunda autoridad de Source.

Python de la distribución vigente: **3.14.2**. Baseline Project objetivo: **3.14.7 / `python:3.14.7-slim-trixie`**. Discrepancia **OPEN / OTHER FOCUS**.

**NEXT ÚNICO:** reconciliar los reemplazos canónicos del cierre 001D con `atlanticus-cannonical:main` y la documentación de decisiones pertinente. Sin implementar nuevas funcionalidades en ese chat.
