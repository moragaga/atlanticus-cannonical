# Alarm Engine — Decision Index

Estado: **CURRENT — inventario documental de decisiones implementadas/refinadas y conflictos pendientes; no crea decisiones formales nuevas en `atlanticus-decisions`**. Lectura 2026-09-28: implementación `a799dc15105d3e037f36ab77129ef0cfa8999013`, decisions `50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`, canonical de partida `46877f174513b2475f17b7dc739cd43951fa4ed0`.

Los identificadores `ALARM-*` de esta tabla son **descriptivos**, no nuevas keys oficiales de decisiones. Consultar `alarm_decisions/R3.6M-006B.1-alarm-definition-contract-inventory-DESIGN-FROZEN.md` y decisiones B.2 para interpretar vigencia histórica.

| Identificador descriptivo | Decisión o frontera | Estado al corte |
|---|---|---|
| ALARM-DEF-B1 | `AlarmDefinition`, Rules/Messages, identidad y parámetros simples | CURRENT como dominio; cambios `evaluator_key`/`kind`/grupo divergentes respecto de intención deseada. |
| ALARM-SOURCE-V3 | Source v3 y Tool manifest exacto | CURRENT; v2 SUPERSEDED sin decoder legacy. |
| ALARM-ROUTING-STRICT | PROCESS -> INTEGRATED_OPERATIONS -> STRATEGIC -> END | CURRENT / FROZEN. |
| ALARM-B2-RESOLVER | Resolver B.2 puro | CURRENT; el paquete incorpora I/O separado. |
| ALARM-MATERIALIZATION-LOCAL | READY/BLOCKED local, pareja Rn/Cn exacta, manifest | CURRENT; salida Cosmos B.2 anterior SUPERSEDED. |
| ALARM-B1-EXACT-REF | `AlarmConfigurationArtifactRef` y planning por identidades definidas | CURRENT. |
| ALARM-B2A-WAL | V1 0 grupos y V2 1..N grupos con referencias exactas | CURRENT; V1 no es legacy. |
| ALARM-B2B-EFFECTIVE | Effective Head reparable + lector exacto | CURRENT. |
| ALARM-B2C-EXECUTION | Adopción/evaluación en sesión fijada con recovery/lease | CURRENT en código; despliegue físico UNVERIFIED. |
| ALARM-B2C5C-SOURCES | Fuentes/particiones registradas, consolidación y entrega individual | CURRENT, 436 pruebas locales del gate compartido. |
| ALARM-B2C5D-CATALOG | Evaluador y requisitos por lógica, registro productivo separado de ejemplos | CURRENT, 443 pruebas locales al cierre. |
| ALARM-B2C6-WIRING | Arranque/composición ejecutable real de puertos ya existentes | PLANNED, debate primero. |
| ALARM-WEB-DEACTIVATION-END-SHIFT | Configurar límite `fin del turno` | OPEN, no decidido ni implementado; frente Web/Domain/Core/calendario distinto de B2c.6. |
| ALARM-QUALIFICATION-REAL | Productores de qualification operacional y despliegue físico | UNVERIFIED / OPEN. |
| ALARM-LIVE-HISTORY | Proyección Live, Management Capture, Analytics | PLANNED / SEPARATE. |

## Genealogía SUPERSEDED y refinamientos

1. **SUPERSEDED:** Source v2 y salida Materialization Cosmos. El flujo actual es Source v3 con Tool manifest exacto; salida READY/BLOCKED en volumen local. Cosmos permanece como entrada de proyección cuando corresponde.
2. **REFINED:** Rn/Cn solos no fijan la qualification ni el artefacto físico. La referencia usa source/result/hash/resolution; READY nunca concede autoridad EFFECTIVE.
3. **REFINED:** B1 planifica unión de identidades definidas; B2a implementa adopción durable mediante WAL existente V1/V2; B2b añade Effective Head y lectura exacta; B2c integra ejecución de sesión/ciclo. Una descripción antigua de 'B2c completo pendiente' está SUPERSEDED en cuanto a esas piezas ya presentes.
4. **REFINED B2c.5c/B2c.5d:** el puerto general de requisitos puede aceptar tupla estática o resolver dinámico. La **convención acordada para nuevas lógicas del catálogo** es que el desarrollador declare los datos manualmente; los parámetros Web, si existen, influyen en evaluación, no en resolver columnas/particiones. La primera propuesta paramétrica B2c.5d quedó SUPERSEDED, **sin borrar** el puerto existente.
5. **SUPERSEDED:** ejemplo registrado automáticamente bajo familia `mina` como si fuese alarma productiva. La referencia actual está en `catalog/examples/threshold` y `catalog/registry.py` retorna cero contratos. No instalar un catálogo real por inferencia.

## Conflictos y OPEN, no reconciliados por este cierre

- **Decisions B.1 vs main:** `evaluator_key`/`kind` deseados compatibles y `priority_group` con migración entre grupos; `adoption.py` sigue rechazando estas mutaciones. No declarar ganadora ninguna interpretación: código es CURRENT; decisión histórica conserva intención pendiente de implementación/acuerdo.
- **Web/Domain vs necesidad expresada:** actualmente `max_duration_hours` entero 1..12 y editor numérico; no hay contrato confirmado de selección fin de turno. No inventar un enum, opción, calendario o algoritmo en este cierre.
- **Conceptual visual targets vs editor:** la independencia conceptual frente a routing continúa en tensión con sincronización existente; debate UX separado.
- **Python:** proyecto objetivo 3.14.7 y paquetes Command Center `==3.14.2`, no alinear incidentalmente.
- **Canonical de partida:** aún narra B2c como pendiente de implementar. Estos archivos son candidatos de actualización y no prueban un push a canonical.
