# Atlanticus — Authority

Estado: **CURRENT / CHECKPOINT ACOTADO AL HITO ALARM MATERIALIZATION, 2026-09-27**

## Fuentes consultadas para este cierre

| Fuente | SHA confirmado | Autoridad |
|---|---|---|
| `moragaga/atlanticus:main` | `b600ca591b56d0924aed752dfae6e9fab2c6f1d6` | Realidad implementada al momento del cierre. |
| `moragaga/atlanticus-cannonical:main` | `772d15078c97802d58d8b658b0d5d5b928fa2ed5` | Baseline documental leído, anterior a la eventual integración de estos reemplazos. |
| `moragaga/atlanticus-decisions:main` | `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e` | Decisiones históricas; interpretar según vigencia, refinamientos y código actual. |

**Alcance de auditoría:** se verificaron el HEAD, rutas y contratos relevantes para Alarm Materialization, no una auditoría completa de todos los módulos de Atlanticus. Los SHA son checkpoints, no garantías sobre futuros cambios. Git permanece **SOLO LECTURA**.

Jerarquía:
1. `atlanticus:main`: implementación efectivamente existente; comprobarla en cada nuevo incremento.
2. `atlanticus-cannonical:main`: estado y contratos documentados; contrastar contra código.
3. Tests/qualification: evidencia restringida al SHA y entorno realmente ejecutados.
4. Decisiones explícitas recientes del Project todavía no materializadas: delta acordado por formalizar.
5. `atlanticus-decisions:main`: decisiones y genealogía; los contratos históricos reemplazados no vuelven a imponerse.
6. Historial conversacional: guía de búsqueda, nunca autoridad por sí solo.

Ante conflicto, **registrarlo**, sin resolverlo silenciosamente.

## Estado de la frontera Alarm Materialization

**VERIFIED / CURRENT en main:** Domain y Core de Alarm; `AlarmConfigurationSnapshot` v3 con `ToolDependencyManifest` exacto; codec y stores de proyección Local/Cosmos; resolver B.2 puro; proceso ejecutable `backend/processes/alarms-materialization` v0.2.1, con adquisición de proyección, proveedor JSON de qualifications, `execute_job`, codecs y publicador **actualmente dirigido a Cosmos**.

**DECIDED / NOT IMPLEMENTED:** el proceso Materialization será el lector Cosmos de la proyección operacional y publicará **artefactos locales de Runtime y Delivery en `VOLUMEN_PATH`**, para consumo local por los jobs. La salida Cosmos actual queda **SUPERSEDED como diseño**, aunque todavía existe físicamente en main. No conservar dos publicadores como legacy.

**No elevar tests a una certificación:** `uv sync --python 3.14.2` y wheel de v0.2.0 constan en evidencia local del usuario; esa misma ejecución informó 11 fallos de fixture (`tool-a`), cinco problemas Ruff de imports y diez archivos por formatear. El HEAD v0.2.1 incorpora la corrección (`tool_a` y formato), pero **no se aportó rerun integrado de pytest/Ruff/format/wheel sobre v0.2.1**. No hay infraestructura Cosmos disponible ni integración real.

**CURRENT / FROZEN:**

```text
LATEST SAVED = LATEST VALID_AT_SAVE
VALID_AT_SAVE != READY != EFFECTIVE
INVALID != REMOVED
DISABLED != INVALID
DISABLED != REMOVED
TRACE_ONLY != REMOVED
READY != EFFECTIVE
```

Tool Catalog consolidado tiene Storage/Blob como destino durable en el dominio migrado; no proyectarlo nuevamente a Cosmos por conveniencia. Alarm Source v3 retiene solamente evidencia Tool exacta Rn/Cn. Web puede usar Cosmos como superficie operacional; Engine y Delivery **no necesitan Cosmos para leer sus contratos materializados**.

`ALARM-ROUTING-STRICT` continúa congelado: `PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC`, sólo próximo nivel, sin saltos, retorno o mismo nivel; Strategic terminal y sin visual target contratado. La controversia entre independencia conceptual de visual targets y la sincronización del editor sigue abierta y no se altera en este hito.

**Diferencia Python abierta:** baseline objetivo del Project `3.14.7` frente a metadatos actuales de paquetes Command Center `==3.14.2`; no realizar correcciones incidentales.

## Límite del siguiente incremento

Sólo terminar la salida LOCAL, inmutable, versionada y publicada coherentemente por Materialization, revisar primero componentes locales existentes, realizar pruebas sin Cosmos real y preservar el resolver puro. No abrir Runtime Adoption, Live Delivery, Manager UX ni infraestructura en este mismo cambio.
