# Alarm Engine — Open Items

Estado: **CURRENT / MATERIALIZATION LOCAL OUTPUT NEXT / NO LIVE COSMOS ENVIRONMENT**

Corte: `atlanticus@b600ca591b56d0924aed752dfae6e9fab2c6f1d6`; canonical `772d15078c97802d58d8b658b0d5d5b928fa2ed5`; decisions `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`.

## CLOSED en cuanto a contrato o existencia de código

- Pure B.2 y atomicidad lógica READY/BLOCKED; `AlarmResolutionKey(Rn,Cn)` compartida.
- Source v3 y manifest Tool exacto congelado con cada release Alarm.
- Proyección de Alarm Configuration: codec y adaptadores Local/Cosmos existentes.
- Strict routing Domain/B.2 y opciones de editor correspondientes, con conflicto visual señalado aparte.
- Decisión de arquitectura del Project: Materialization obtiene el input Cosmos; Runtime/Delivery consumen B.2 local desde volumen; READY no es EFFECTIVE.
- Proceso ejecutable Materialization v0.2.1 **existe en Git**. Esto **no** cierra su validación ni la salida local todavía no implementada.

## NEXT / ÚNICO FOCO — MATERIALIZATION LOCAL OUTPUT

**PLANNED:** sustituir el publicador de resultados Cosmos en `backend/processes/alarms-materialization` por publicación local coherente, sin dual writes ni adaptador legacy. Contrastar mecanismos existentes de archivos/estado antes de fijar interfaces.

Tareas de ese único incremento, en orden:

1. **VERIFIED BEFORE CHANGING:** comprobar HEAD y árbol del proceso 0.2.1, el resolver, `atlanticus.runtime`, opciones reales de persistencia/atomicidad local y pruebas. Verificar qué contrato de lectura local ya existe para evitar otro nuevo.
2. **CONTRACT BEFORE CONSUMER:** especificar contrato de artefacto por versión exacta, manifest/procedencia/hashes, publicación coherente, BLOCKED/diagnósticos e idempotencia; decidir rutas/nombres sólo con evidencia del volumen actual. No introducir marcador EFFECTIVE propiedad de Materialization.
3. **INCREMENTAL IMPLEMENTATION:** reemplazar la salida Cosmos y settings asociados; mantener acquisition Cosmos, resolver puro y proveedor JSON controlado mientras sus productores reales estén OPEN. Mantener espejo comentado equivalente. Entregar únicamente archivos modificados y tests.
4. **LOCAL ACCEPTANCE:** re-ejecutar tests/ruff/format sobre dependencias reales; agregar pruebas de escritura/lectura coherente, deduplicación, contenido alterado, crash antes de publicar READY, retry sin efectos parciales, BLOCKED sin artefactos ejecutables, divergencia de revisions y cambio de proyección/evidencia. Usar mocks/stores locales ya existentes; no exigir Cosmos real para este gate.
5. **EVIDENCE/STOP:** registrar resultados del usuario y detener el incremento tras salida local verificable. No abrir automáticamente integración de Runtime ni proveedores GREEN.

## OPEN separados y motivo

| Frente | Estado | Por qué abierto | Cuándo tocarlo |
|---|---|---|---|
| Rerun proceso 0.2.1 | IN PROGRESS / UNVERIFIED | No hay resultado local posterior a corrección sobre dependencias reales. | Primera verificación del próximo foco. |
| Tool GREEN producer | OPEN / UNVERIFIED | Proveedor operacional exacto no auditado; JSON actual es manual/controlado. | Otro incremento, si el flujo real lo requiere. |
| Evaluator qualification producer | OPEN / UNVERIFIED | Registry/despliegue real no contrastado. | Otro incremento. |
| Cosmos/Blob operacional E2E | BLOCKED / UNVERIFIED | Usuario no dispone aún de la infraestructura. | Cuando exista entorno; no bloquear tests locales. |
| Layout atómico/multiinstancia del volumen | OPEN / PROPOSED | No se inspeccionó el host/FS/volumen concreto ni API local reutilizable definitiva. | Definir al comienzo del foco local; no suponer rename multi-FS. |
| Runtime local reader / Adoption/Effective Head | PLANNED | Debe consumir la salida local congelada, no dictar su contrato prematuramente. | Después de cerrar Materialization local. |
| Delivery local reader + Live publication | PLANNED / SEPARATE | No es un consumidor de prueba del incremento actual; necesita Engine current state y effective exacto. | Después de Runtime Adoption, en otro foco. |
| UI-host end-to-end | UNVERIFIED | No hay evidencia del ciclo real Manager->Cosmos->Materialization. | Al disponer de infraestructura. |
| Routing/visual synchronization | CONFLICT | Canonical separa decisiones visual/routing, editor sincroniza targets. | Otro debate explícito. |
| Source v2 durable existente | UNVERIFIED | No se ha inventariado la base real; v2 no tiene lector legacy. | Antes de rollout a datos preexistentes, si aplica. |
| Python target 3.14.7 vs metadata 3.14.2 | OPEN / SEPARATE | Cambio transversal fuera de Materialization. | Frente dedicado. |

No repetir campañas R3.5 ni modificar contratos del Engine por una falla que sólo afecte fixtures locales.
