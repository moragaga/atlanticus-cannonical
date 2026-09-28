# Web Platform — Open Items

Estado: **CURRENT / USERS RECOVERY AND MANAGER UI CLOSED FOR VALIDATED SCOPE / MASTER PROJECTION NEXT**  
Corte estático: `moragaga/atlanticus@208c8d6244795ba92cbe6f8e6b11e9743191367d`. Decisiones consultadas: `atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`. Evidencia operacional parcial comunicada por usuario, no CI del commit de corte.

## CLOSED en el alcance demostrado

- **Users Recovery backend**: snapshots inmutables de promovidos, digest, validación, RESTORE estricto, REPLACE de registro/usuarios, antes de imagen y auditoría. **VERIFIED STATIC + VERIFIED USER-REPORTED** en tests y laboratorio Cosmos/Azurite; ver `13_USERS_PROJECTION_RECOVERY.md`.
- **Users Projection dentro del Manager**: `ManagerEntry` genérica, pestañas Crear respaldo/Proyectar usuarios, resumen histórico con fecha y metadatos, comparación, modal de confirmación; correctivos 004–007 integrados en `208c8d6`. UI y captura aceptadas por el usuario; no confundir con calificación de operación invasiva desde navegador.
- **Configuración de ámbito**: se eliminaron variables redundantes de Users Recovery en la UI del Manager; se reutilizan conexión, `application_namespace` y autorización existentes. El `tool_namespace` no subdivide automáticamente usuarios.
- Los cierres anteriores de Starter/Compose, Navigation y Manager permanecen **históricos** con sus respectivas evidencias; no extender sus resultados a la versión actual sin pruebas.

## OPEN y razón

| Ítem | Estado | Razón |
|---|---|---|
| `MASTER-PROJECTION` página externa | PLANNED / NEXT | Contrato de acceso aislado, plan y ejecución todavía sin código verificado. |
| Material de acceso generado por tooling ADA y consumido por warmup | PLANNED / DESIGN GATE | Formato, generador, integridad, cifrado/verificación y ubicación de integración aún no acordados. Reusar generadores existentes. |
| Página con material ausente | PLANNED / ACCEPTANCE | Debe informar «acceso no configurado» y no permitir autenticación ni proyección, sin excepción innecesaria. |
| Página con material válido | PLANNED / ACCEPTANCE | Autenticar usuario/contraseña, inspeccionar Sources/proyecciones aplicables y permitir acciones autorizadas/reintentos. |
| Pre-Manager con acceso de servicio vs exigencia Entra anterior | OPEN / CONTRACT CONFLICT | Delimitar autorización independiente y protección productiva sin reemplazar Identity normal del Manager. |
| Bootstrap Users en target sin promovidos | OPEN / DESIGN GATE | Provider de la UI Manager deriva `identity_realm` de promovidos; no sirve como provider del target vacío. |
| Parciales/recuperación real de REPLACE | OPEN / SECURITY AND RELIABILITY | Sin atomicidad Blob/Cosmos ni rollback. Faltan prueba con interrupción/reintento y aislamiento real. |
| Revocación efectiva de sesiones y acciones administrativas | OPEN / SECURITY GATE | Confirmaciones manuales no prueban revocación ni mantenimiento. |
| UI RESTORE/REPLACE invasivos | UNVERIFIED | Backend REPLACE validado en Docker mediante CLI, no prueba destructiva desde navegador. |
| Transporte y compatibilidad interambientes | OPEN | Se requiere distribución explícita de snapshot aprobado; no inferir compatibilidad entre distintos `issuer/subject_id`. |
| Ruff final, full-monorepo CI, producción Entra/Azure | UNVERIFIED / SEPARATE | No se aportó qualification completa para commit `208c8d6`. |
| Datos operacionales ADA por usuario | PLANNED / OTHER FOCUS | Cargo/área/grupo no pertenecen a Users genérico ni son necesarios para Master actual. |
| `extra` de Users/Cosmos | DEFERRED IDEA | No forma parte de este cierre ni autoriza crear un contrato nuevo. |
| Desfase Python Project 3.14.7 / uso puntual de 3.14.2 | OPEN / SEPARATE | Mantener registro de versiones realmente probadas; no cambiar baseline durante Master. |

## Prioridad única

**`MASTER-PROJECTION-001`: diseño y contrato de página externa de proyección + archivo/material generado usando tooling existente.** Dos estados obligatorios: material ausente → información y ninguna acción; material válido → autenticación de servicio y proceso autorizado de despliegue de las proyecciones disponibles. Primero verificar el código actual de tooling/Starter/loader e identificar contratos de proyección existentes y dependencias; sólo tras consenso implementar incrementalmente. No mezclar ADA usuarios operacionales, alarmas, KPI, rediseño de Manager, ni refactors generales.
