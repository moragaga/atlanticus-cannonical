# Web Platform — Open Items

Estado: **CURRENT / FOCUS NEXT: USERS-PROJECTION-RECOVERY-001**

Corte estático: `moragaga/atlanticus@ebc7a8bf8d49e931fd4e2487dac5ee036011a0a5`. Evidencia de ejecución en checkpoints de este hito proporcionada por usuario; no es qualification global del HEAD. No mezclar el trabajo paralelo de Alarm Engine o Command Center.

## CLOSED / CURRENT acotado

- ADA Generic Core Stage 1, Collector y Manager local, según checkpoints anteriores.
- SOURCE_SMOKE/PORTABLE Web históricos bajo Python 3.14.2.
- Starter ADA Compose `full`: tests de tooling/Compose **33 passed** reportados; generación/build/arranque local con Cosmos vNext, Azurite, recursos y Gunicorn comprobados por consola y usuario.
- Patch de etiquetas del Manager aplicado con checks Git limpios y regresiones reportadas (once puntos pytest más seis aprobados). Visual desde artifact nuevo no demostrado.

## OPEN explícitos

| Elemento | Estado | Por qué |
|---|---|---|
| Conjunto aprobado de Users en Source | PLANNED / NEXT | `UsersRegistrySnapshot` también puede contener candidatos; la sola presencia en Blob no acredita promoción. |
| Users validar/reproyectar Cosmos | PLANNED / NEXT | `discover` muestra conflictos; no existe recuperación integral invasiva ni política de registros inesperados. |
| Source y Cosmos difieren tras fallo parcial | OPEN / NEXT | Blob `replace` antecede create/replace Cosmos; no hay transacción distribuida. |
| Sesiones con permisos previos tras recovery | OPEN / SECURITY GATE | Reconstruir Cosmos no invalida automáticamente sesiones existentes. |
| Comparación/migración interambientes | OPEN | ETag local no es versión portable; directorio Entra puede variar y requiere correspondencia autorizada. |
| Página aislada de proyección y tooling de credenciales | PLANNED / AFTER USERS | Debe operar sin perfiles locales de destino y no dar Manager access. Flujo aceptado; implementación y contrato criptográfico OPEN. |
| Módulo ADA de cargo/área/grupo | PLANNED / AFTER PAGE | Nuevos datos de dominio opcionales; Source/Projection y topología `users-support` aún no implementados. |
| `extra` futuro en Cosmos / Access ampliado | DEFERRED IDEA | No crear contrato ni modificar Users ahora. |
| Provider labels visual tras nueva imagen | UNVERIFIED / NONBLOCKING | Checks y tests realizados, sin captura/validación final del nuevo artifact. |
| Durable data restart E2E | UNVERIFIED / SEPARATE | Se verificó arranque; falta evidencia específica de lecturas después de `down/up`. |
| Entra productiva y claves/secrets/infrastructure ownership | PLANNED / UNVERIFIED | Host productivo real no integrado en Starter; credenciales de servicio no especificadas. |
| Python Project 3.14.7 vs Web 3.14.2 | OPEN / SEPARATE | Desfase objetivo/metadata/imagen observado; no corregir fuera de alcance. |
| CI global/full test suite y servicios Azure | UNVERIFIED | Sólo pruebas puntuales y entorno local reportados. |

## Prioridad única

`USERS-PROJECTION-RECOVERY-001`: auditar `UsersRegistrySnapshot`, operaciones `discover/promote/update`, Blob/Cosmos stores y namespace en HEAD vigente; definir primero criterio de usuario explícitamente aprobado, validación sin mutación y recuperación invasiva controlada de Cosmos. Definir fallos parciales, concurrencia, desconocidos y seguridad de sesiones. **Sólo después de consenso** autorizar incremento backend/tests + espejo comentado.

No abrir en el mismo incremento página de despliegue, generación ZIP, frontend de cargos ni cambios de layout de Manager.

Contrato detallado de handoff: `13_USERS_PROJECTION_RECOVERY.md`.
