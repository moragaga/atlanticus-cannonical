# Alarm Engine — Open Items

Estado: **CURRENT / ALARM CONFIGURATION & STRICT ROUTING IMPLEMENTED / MATERIALIZATION JOB NEXT**

Checkpoint de código auditado: `moragaga/atlanticus@411aea44ac60c09d2b07ce41d34c3f378788b97b`.

## CLOSED / CURRENT en código

- Pure B.2 resolver y contratos `AlarmConfigurationResolution`, Runtime/Delivery y atomicidad READY/BLOCKED.
- Qualification explícita de Tool/Evaluator como inputs del resolver.
- `domain/tools`, `ToolDependencyManifest`, `AlarmConfigurationSnapshot` v3 y correlación exacta Rn/Cn.
- Freeze de references incluyendo Rules inactivas y steps deshabilitados.
- Alarm Source/base Projection, Local/Cosmos projection stores y composición de proveedores.
- Política de escalamiento siguiente nivel en Domain y validación B.2.
- Editor Web con opciones direccionales y separación Strategic routing / visual.
- Suites de Domain, Materialization y Web Alarm Configuration **VERIFIED** en la ejecución aportada durante el hito, antes del commit `411aea...`.

## 1. Materialization Process / job — PLANNED / NEXT

Objetivo solicitado: desde configuración ya authored/publicada, **adquirir la proyección operacional exacta**, obtener qualifications, ejecutar el resolver puro, persistir/poner a disposición los artefactos y preparar su descarga/lectura para Runtime. Primero contrastar el contrato existente; no inventar stores ni deployment.

OPEN de contrato y evidencia:
- modo de acceso efectivo a la proyección operacional y referencia exacta `ProjectionRecord[AlarmConfigurationSnapshot]`;
- prueba del productor/consumidor real de Cosmos/Blob: adapters presentes, end-to-end no observado;
- productores e inputs de Tool GREEN y evaluator qualification;
- storage, IDs, schema/codec, provenance y publicación de Runtime/Delivery/findings;
- trigger manual/semi/automático, reintentos, errores, observabilidad y permisos, sin asumir automatización completa;
- contrato concreto de descarga/lectura para el posterior Runtime, verificando ownership existente.

No abrir simultáneamente Runtime Adoption ni Live Delivery.

## 2. Tool reconciliation qualification producer — OPEN

De dónde viene evidencia GREEN y cómo se entrega sin duplicar reconciliación Tool.

## 3. Evaluator qualification producer — OPEN

Contrastar el deployed evaluator registry/catalog y su frontera. No inventar registry.

## 4. Artifact stores / operator output — OPEN

Quién persiste Runtime/Delivery/findings, con qué revision/exact key y cómo se recupera; no confundir resolución pura con persistencia.

## 5. UI-host / operational E2E gate — UNVERIFIED

Faltan prueba host/browser posterior al routing y ciclo real guardado -> publicación -> Cosmos -> materialización; los tests de componente no sustituyen esa evidencia.

## 6. Routing versus visual-target derivation — CONFLICT A CLARIFICAR

Canonical de UX indica que un destino visual **no equivale** a un destino de routing y no deben condicionarse sin decisión explícita. En main, `synchronize_visual_targets` deriva visual targets desde origen y pasos de routing habilitados, aunque excluye Strategic. No cambiar silenciosamente ninguno durante el job.

## 7. Runtime provenance cleanup / Runtime Adoption / Effective Head — PLANNED AFTER

`READY != EFFECTIVE`. No hay Effective Head ni Adoption Commit implementados en este corte. No mezclar con Materialization.

## 8. Live Delivery, Management Capture y Analytics — PLANNED / SEPARATE

Fuera del siguiente foco.

## 9. V2 durable releases — UNVERIFIED

Schema v2 SUPERSEDED sin legacy decoder. Si hay releases durables v2 en ambientes reales, evaluar reset/migración **antes** de ejecutar un job que requiera v3. No introducir decoder preventivo.

## 10. Python baseline — OPEN / SEPARATE

Project `3.14.7` frente a packages Command Center `==3.14.2`. No cambiar metadata incidentalmente.

## Siguiente foco único

**Contrato operacional e implementación incremental del job B.2 de Materialization**, comenzando por evidencia de la proyección y de las qualifications/outputs existentes. No escribir código ni configurar infraestructura sin el análisis del siguiente chat.
