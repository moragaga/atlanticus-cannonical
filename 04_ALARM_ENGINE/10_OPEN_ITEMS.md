# Alarm Engine — Open Items

Estado: **CURRENT / MATERIALIZATION LOCAL + RUNTIME PLAN B1 CLOSED LOCALMENTE / GLOBAL ADOPTION NEXT**

Corte: `atlanticus@c8f23d91ae1cb817be55b4b812b22ffca518880e`; canonical anterior `58241ddb6db5adbd2e783c7ec9f456f1bda5a321`; decisions `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`.

## CLOSED para este cierre

- Source v3 y `ToolDependencyManifest(Cn)` exacto en Rn; B.2 determinista y pareja lógica Runtime/Delivery coherente.
- Proceso Materialization: adquisición Cosmos de la proyección de entrada, qualification controlada, salida local versionada READY/BLOCKED, manifest/hash, puntero READY e idempotencia probada localmente. Sin salida Cosmos duplicada ni codec privado anterior.
- Lector compartido y `RuntimeLocalConfigurationReader` para READY actual y versión exacta fijada mediante `result_id`/manifest SHA256.
- **B1:** `AlarmConfigurationArtifactRef`, revisión desde candidato READY y registro de evaluadores explícito; planificación sobre unión de identidades definidas con `ADDED`/`ENABLED`.
- **Validación local:** A y B1 cuentan con tests unitarios, Ruff y wheels, según logs entregados por el usuario; el código está integrado en el SHA de `atlanticus` citado. No adjudicar CI/E2E que no se ejecutó.

## OPEN — por qué siguen abiertos

| Frente | Estado | Motivo concreto | Frontera |
|---|---|---|---|
| Revisión/ejecución de plan B1 | **PLANNED / OPEN** | `is_adoptable` no implica ejecutabilidad: ADDED/ENABLED y REMOVED desde una Rule previamente deshabilitada activan `requires_execution_upgrade`; ejecutor actual requiere source plan ejecutable. | Diseño de Runtime Adoption, siguiente foco técnico. |
| Adopción global durable / Effective Head | **PLANNED / OPEN** | WAL actual tiene `EngineCommitRecord` por grupo; no existe commit global de configuración ni efectiva durable para cambios sin mutación de grupos, como Delivery-only. Ubicación/formato/publicador/recovery no aprobados. | Mismo debate de diseño siguiente, implementación sólo tras consenso en incrementos aislados. |
| Bootstrap y selección efectiva tras recuperación | **PLANNED / OPEN** | No hay un global Effective Head persistido que permita reconstruir inequívocamente qué `result_id`+SHA256 se adoptó tras reinicio. No usar latest READY como sustituto. | Runtime Adoption durable. |
| Ejecución `ADDED`/`ENABLED`/REMOVED deshabilitada | **PLANNED / OPEN** | B1 las clasifica pero no reconcilia/persiste operacionalmente. Requiere revisar semántica de grupo y flujo de adopción sin crear adaptador legacy. | Runtime Adoption; primero contrato. |
| Divergencia evaluator/kind/priority_group | **OPEN / CONFLICT** | Decisiones B.1 desean compatibilidad/migración; implementación aún rechaza esos cambios. No ampliar política dentro de cierre B1. | Debatir explícitamente antes de habilitar esos casos. |
| Reappearance sobre ManagementEffect vigente | **OPEN** | La semántica de cambio de timer/condiciones especiales en una occurrence ya gestionada necesita reconciliación verificable. | Posterior incremento de Adoption. |
| Proveniencia de occurrence `resolution_key_at_start` | **PLANNED** | Código actual conserva revisiones de Alarm/Tool separadas; modelo objetivo no implementado. | Posterior incremento dedicado, si se confirma contrato. |
| Productores reales Tool GREEN y evaluator qualification | **OPEN / UNVERIFIED** | Hay proveedor JSON controlado y registro de evaluadores explícito para pruebas; no se ha comprobado pipeline operacional real. | Otro incremento de productores/integración. |
| Materialization E2E y volumen definitivo multi-host | **UNVERIFIED / BLOCKED por entorno** | Unit tests no prueban infraestructura Cosmos/Blob ni semántica de rename/fencing sobre el volumen real compartido. | Cuando exista host e infraestructura adecuados; no bloquear el debate B2. |
| Delivery local exacto, Live y Management Capture | **PLANNED / SEPARATE** | Necesitan la identidad EFFECTIVE real; no deben adoptar latest READY ni determinar autoridad del Engine. | Después de Runtime Adoption durable. |
| Visual targets vs editor de routing | **OPEN / CONFLICT** | Independencia conceptual y sincronización actual del editor no están reconciliadas. | Debate UX/routing separado. |
| Source v2 durable real | **UNVERIFIED** | No se ha inventariado datos reales previos; no existe lector legacy contratado. | Antes de rollout a datos antiguos, sólo si aplica. |
| Python `3.14.7` vs `==3.14.2` en Command Center | **OPEN / SEPARATE** | Política base del Project distinta de metadata del código vigente; cambios transversales fuera de este hito. | Frente exclusivo si se prioriza. |
| Tests/CI sobre checkout limpio de `c8f23d9` | **UNVERIFIED** | Los PASS proceden de logs locales del árbol de trabajo antes de commit, aunque se constató que HEAD incorpora B1. | Opcional gate de distribución/CI, sin reabrir B1 local. |

## Una sola frontera técnica para el siguiente ciclo

**PROPOSED — Runtime Adoption durable: debate/diseño primero.** Partir de `adoption.py`, `adoption_execution.py`, `job_composition.py`, `composition.py`, `durability.py`, `alarms/persistence/{models,store}.py`, `local_configuration.py` y el contrato B1; verificar de nuevo HEAD antes de actuar.

Secuencia **conceptual**, sin ejecutar ahora:

1. Contrastar disposiciones B1, `requires_execution_upgrade` y capacidad de reconciliación actual, incluido source/target definido y vacío.
2. Acordar cómo representar en el **WAL existente** una adopción global vinculada al artefacto exacto, incluso con cero commits de grupo; no crear grupo sintético ni journal paralelo.
3. Acordar el orden de autoridad/persistencia/recovery del futuro Effective Head frente a `WAL -> DURABLE HEAD -> SNAPSHOTS -> MATERIALIZED HEAD`, con fencing y crash points reales.
4. Dividir la implementación acordada en incrementos pequeños y pruebas unitarias de contratos, recovery, idempotencia y fallos. Mantener paquetes del frente en `1.0.0` hasta distribución.
5. Sólo después planificar Delivery/Live en otro frente.

**En este cierre no se implementa nada.** El paso inmediato de gestión es integrar los reemplazos canónicos preparados, validar su diff y obtener el nuevo HEAD de canonical; luego abrir el foco técnico único anterior. Git permanece SOLO LECTURA desde el asistente.
