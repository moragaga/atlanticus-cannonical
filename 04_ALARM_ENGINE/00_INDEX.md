# Alarm Engine — Index

Estado: **CURRENT / PURE B.2 AND EXECUTABLE MATERIALIZATION v0.2.1 / LOCAL OUTPUT REDESIGN NEXT**

Checkpoint: `atlanticus@b600ca591b56d0924aed752dfae6e9fab2c6f1d6`; canonical base `772d15078c97802d58d8b658b0d5d5b928fa2ed5`; decisions `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`.

| Archivo | Responsabilidad | Situación de este corte |
|---|---|---|
| `01_DOMAIN_MODEL.md` | Dominio/Core y formas de Runtime. | CURRENT; sin cambios aquí. |
| `02_RUNTIME_AND_LIFECYCLE.md` | Lifecycle, prioridad y routing. | CURRENT; sin cambios aquí. |
| `03_PERSISTENCE_AND_RECOVERY.md` | WAL y recuperación operacional del Engine. | CURRENT; no aplicar automáticamente ese protocolo al publicador de Materialization. |
| `04_CONCURRENCY_LEASES_AND_FENCING.md` | Authority y fencing. | CURRENT; contrastar antes de publicar en volumen. |
| `05_PROJECTION_AND_PUBLICATION.md` | Source/base/Cosmos operacional vs configuraciones B.2 y proyecciones Live/Management. | ACTUALIZADO. |
| `06_MANAGEMENT.md` | Management y deactivation. | CURRENT; sin cambios aquí. |
| `07_CONFIGURATION_AND_MATERIALIZATION.md` | Pipeline, implementación presente y cambio a volumen. | ACTUALIZADO. |
| `08_QUALIFICATION_BASELINE.md` | Campañas anteriores y límites de evidencia de este proceso. | ACTUALIZADO sin reinterpretar campañas históricas. |
| `09_DECISION_INDEX.md` | Estado de decisiones refinadas y conflicto registrado. | ACTUALIZADO. |
| `10_OPEN_ITEMS.md` | Pendientes de esta frontera y tareas posteriores separadas. | ACTUALIZADO. |
| `11_SOURCE_LEDGER.md` | SHA, rutas y evidencias verificadas/no verificadas. | ACTUALIZADO. |
| `12_COMMAND_CENTER_ANALYTICS_BOUNDARY.md` | Frontera Engine/Analytics. | SEPARATE; sin cambios. |
| `13_RUNTIME_ADOPTION_AND_EFFECTIVE_CONFIGURATION.md` | Lectura por identidad exacta y adopción posterior. | ACTUALIZADO con diferencia entre helpers existentes y Effective Head global pendiente. |

## Cadena de responsabilidad acordada

```text
Alarm Source Rn + ToolDependencyManifest(Cn) -> ProjectionRecord en Cosmos
-> Materialization adquiere candidato exacto + qualifications -> B.2
-> READY: Runtime + Delivery de la misma AlarmResolutionKey en VOLUMEN_PATH
-> posterior Runtime Adoption -> EFFECTIVE
-> Engine/Delivery leen archivos de su materialización adoptada, sin consultar Cosmos para esa configuración
```

**En código actual:** Materialization todavía escribe un resultado monolítico Cosmos. No confundir el flujo acordado con un despliegue implementado. El siguiente foco es únicamente reemplazar esa salida por archivos locales comprobables y aplicar tests sin infraestructura remota.
