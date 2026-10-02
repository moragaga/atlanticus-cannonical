# Alarm Engine — Domain Model

Estado: **DESIGN FROZEN para el modelo de negocio / OWNERSHIP Y MATERIALIZATION BOUNDARY OPEN**

Fuente principal histórica:
`alarm_decisions/R3.6M-006B.1-alarm-definition-contract-inventory-DESIGN-FROZEN.md`

## 1. Estado y alcance

El modelo de dominio de Alarmas permanece congelado en sus conceptos e invariantes principales.

Lo que está abierto no es la semántica de Rule/Occurrence/Episode, sino:

- ownership físico del Engine;
- dirección de dependencias entre Engine, Command Center y Web;
- frontera exacta entre Resolution y Materialization;
- contrato físico publicado en Cosmos que alimentará Materialization.

No reabrir el modelo de negocio para resolver estos puntos de arquitectura.

## 2. Ownership

### Histórico

Owner histórico del contrato:
`ada-command-center-alarms-core==1.0.0`.

La decisión frozen histórica ubica físicamente Alarm Engine bajo `scopes/ada-command-center/backend/`.

### Estado actual

`VERIFIED / CURRENT`

La implementación ya contiene responsabilidades de engine diferenciadas:

- alarms/core;
- alarms/materialization;
- alarms/persistence;
- alarms-runtime;
- alarms-materialization process;
- alarms-delivery process.

### Dirección propuesta

`PROPOSED / PLANNED`

Alarm Engine debe evaluarse como engine autónomo del ecosistema ADA, consumido por ADA Command Center, en vez de ser propiedad arquitectónica de la Web o del backend específico de Command Center.

La extracción física no debe hacerse antes de cerrar la frontera de Materialization y eliminar dependencias invertidas hacia Web.

## 3. Regla de dependencias

Core/domain del Engine no debe conocer:

- Dash o Flask;
- geometría UI;
- callbacks;
- sesiones web;
- módulos de Configuration Manager;
- implementaciones de proyección Web;
- `ada.web.tools` como dependencia necesaria del Engine;
- clientes de Cosmos/SharePoint/Key Vault dentro del dominio puro;
- WAL/leases/persistencia física dentro del dominio puro;
- detalles CSS o tokens visuales.

Una key utilizada para direccionamiento (`tool_key`, `component_key`, `subcomponent_key`) es dato contractual y no convierte al Engine en consumidor de la implementación Web que conoce esa key.

## 4. Conceptos de dominio congelados

- Rule: alarma configurada.
- AlarmDefinition: definición editable canónica de una Rule.
- PlannedAlarm: forma operacional resuelta para ejecución.
- Occurrence: activación concreta de una Rule.
- Episode: lifecycle compartido dentro de un `priority_group`.

## 5. Identidad

`AlarmIdentity(family_key, alarm_key)`

Invariantes:

- `alarm_key` es estable;
- no reintroducir `rule_key` como identidad paralela;
- `rule_name` es editable y único dentro de family;
- `display_name` es requerido;
- `title` es estático;
- `cause_template` admite materialización dinámica.

## 6. Configuración de negocio

- kind: `RISK | IMPACT`;
- criticality: `C1 | C2 | C3`;
- categorías: Ecology / Productivity / Safety / Costs;
- áreas: Mine / Plant, una o más;
- color semántico: `RED | YELLOW`;
- evaluator: `evaluator_key` + parámetros simples `str | float | bool`;
- no código/listas/nested/None en parámetros;
- enteros numéricos expresados como float.

## 7. Estado de configuración

Una Rule `inactive` sigue definida, pero sale de la ejecución activa.

Si existía una occurrence abierta, el Runtime debe reconciliarla conforme al contrato de configuración deshabilitada sin resetear toda la family o el priority group.

## 8. Visibilidad

- `VISIBLE`
- `TRACE_ONLY`

`TRACE_ONLY` se evalúa y deja trazabilidad, pero no se publica como alarma operacional visible.

## 9. Priority y Special Condition

- `priority_group`;
- `priority_order` positivo y único dentro del grupo.

Una Special Condition es una Rule normal con flag especializado. Su efecto especial opera dentro de la misma family + priority_group conforme al contrato frozen.

## 10. Reappearance

`Reappearance(after_minutes, special_conditions)`

Reaparece si:

- la condición principal sigue activa y vence el timer; o
- se activa una Special Condition referenciada conforme al contrato.

Un cambio de `after_minutes` recalcula el vencimiento. No confundir reappearance de una Special Condition deactivated con reappearance residual de una Rule normal.

## 11. Frontera Resolution → Materialization

`OPEN / CONFLICT WITH RECORDED DECISION`

La decisión histórica B.2 hace que Alarm Materialization participe en adquisición de candidato, Confirmed Tool Catalog y resolución cross-tool.

La dirección acordada en el Project para el siguiente hito es más estricta:

```text
ADA Command Center Configuration
    authoring
    + Tool catalog
    + cross-tool validation
    + destination resolution
        ↓
Resolved Alarm Configuration
        ↓ publish
Cosmos
        ↓
Alarm Engine Materialization
```

Bajo esta dirección, Alarm Materialization no vuelve a descubrir Tools ni re-resuelve relaciones ya publicadas.

Este cambio requiere una decisión formal que reemplace/refine B.2 antes de considerarse frozen en `atlanticus-decisions`.

## 12. Responsabilidad propuesta de Alarm Materialization

`PROPOSED / PLANNED`

Materialization debe:

1. leer una configuración operacional de alarmas ya resuelta y publicada;
2. validar el contrato de entrada propio del Engine;
3. producir dos artefactos coherentes con la misma revisión/resolution key;
4. no depender de implementación Web para interpretar el documento.

Salida conceptual:

```text
Resolved Alarm Configuration
        ↓
Alarm Materialization
        ├── runtime.json
        └── delivery.json
```

## 13. runtime.json

Debe contener solo lo necesario para ejecución del Engine, por ejemplo:

- resolution/revision identity;
- alarm identities definidas;
- planned alarms;
- evaluator keys;
- parameters;
- priority/lifecycle inputs;
- routing operacional requerido por Runtime;
- reappearance/deactivation inputs de ejecución.

Runtime no debe necesitar Configuration Manager ni Web para adoptar esta configuración.

## 14. delivery.json

Debe contener solo lo necesario para enriquecer y despachar los hechos producidos por Runtime, por ejemplo:

- identity;
- display name;
- title/cause contract;
- kind/criticality/category/areas;
- color semántico;
- messages/capabilities de delivery;
- visual targets ya resueltos mediante keys estables;
- `tool_key`;
- `component_key` / `component_keys`;
- `subcomponent_key` y owner cuando corresponda;
- modos de proyección solo si alteran el comportamiento real de Delivery.

No debe transportar geometría UI ni estilos.

## 15. Tool kinds y routing

`OPEN`

Si `ToolConfigurationKind` solo participa en validación de direccionalidad durante authoring/resolution, no debe formar parte del contrato necesario del Engine después de publicada la configuración resuelta.

Debe verificarse en el próximo hito si existe alguna semántica Runtime/Delivery que realmente dependa de `tool_kind`. Si no existe, se elimina de la frontera del Engine y queda solo la key de destino.

## 16. Diferencia estructural con KPI

No copiar el patrón de KPI Collector de forma literal.

### KPI

KPI Delivery distribuye datos por destinos/componentes y la Web mantiene stores por componente.

### Alarm

Alarm Delivery publica un conjunto operacional global de alarmas con visual targets. La Web debe combinar ese conjunto con su `ToolStructure` y construir un layout completo desde un único store/modelo global de alarmas.

```text
Alarm Live Projection
        +
ToolStructure
        ↓
Web Alarm Layout Resolver
        ↓
UN store/modelo global
        ↓
layout completo
```

El Engine determina **qué elemento lógico debe afectarse** mediante keys y semántica de negocio.

La Web determina **dónde está físicamente ese elemento y cómo se representa**.

## 17. Color

Engine entrega color semántico:

- `RED`
- `YELLOW`

Web resuelve representación visual concreta:

- CSS;
- theme token;
- borde;
- background;
- opacity;
- animación.

No mover estilos al Engine.

## 18. Open items

- contrato exacto del documento Cosmos `Resolved Alarm Configuration`;
- ownership del publicador de ese documento;
- reconciliación formal con B.2 registrada;
- eliminación de dependencias backend → `web`;
- necesidad real o no de `ToolConfigurationKind` dentro de Engine;
- naming/namespace final de un posible `ada-alarm-engine`;
- extracción física fuera de `ada-command-center`;
- read model History/Analytics posterior.
