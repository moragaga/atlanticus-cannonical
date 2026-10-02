# Alarm Engine — Command Center / Web / Analytics Boundary

Estado: **CANDIDATE REFINED / OPEN RECONCILIATION**

## 1. Principio

Alarm Engine produce y mantiene estado/hechos operacionales.

Command Center configura, valida y resuelve referencias necesarias para publicar configuración operacional.

Web consume proyecciones operacionales; no participa en resolución del Engine ni lee WAL directamente.

Analytics construye read models históricos; no modifica estado del Engine.

## 2. Dirección conceptual

```text
ADA Command Center Configuration
        ↓
Resolved Alarm Configuration
        ↓
Alarm Engine Materialization
        ├── runtime.json
        └── delivery.json
        ↓
Alarm Runtime
        ↓ durable operational facts
Alarm Delivery
        ↓
Live Alarm Projection
        ↓
Command Center Web
```

En paralelo:

```text
Alarm Engine durable facts
        ↓
History / Analytics read model
        ↓
Command Center analytics surfaces
```

## 3. Ownership histórico y conflicto

### Histórico

La documentación anterior indicaba:

> Alarm Engine pertenece al backend de ADA Command Center.

### Estado actual

`CONFLICT / OPEN`

La implementación ha madurado hacia un Engine con responsabilidades propias, pero aún vive físicamente bajo `scopes/ada-command-center/backend` y conserva dependencias hacia módulos `web`.

La dirección propuesta es que ADA Command Center sea consumidor/configurador del Engine, no su frontera arquitectónica obligatoria.

Este cambio de ownership físico debe formalizarse antes de mover código.

## 4. Frontera Command Center Configuration → Engine

Command Center puede conocer:

- Tool catalog;
- Tool kinds;
- Tool topology;
- components/subcomponents;
- authoring;
- validación cross-tool;
- reglas de direccionalidad;
- resolución de referencias;
- publicación de configuración operacional resuelta.

Alarm Engine no debe necesitar conocer cómo se descubrieron o administraron esas referencias.

El punto de corte propuesto es un documento autosuficiente:

`Resolved Alarm Configuration`.

## 5. Alarm Materialization

`PROPOSED / PLANNED`

Materialization es backend del Engine y su responsabilidad propuesta es:

```text
Cosmos / Resolved Alarm Configuration
        ↓
contract validation
        ↓
materialization
        ├── runtime.json
        └── delivery.json
```

No debe volver a:

- descubrir Tools;
- consultar el catálogo Web de Tools;
- importar Configuration Manager;
- importar proyecciones Web de Alarm Configuration;
- depender de `atlanticus.web.projection` o `atlanticus.web.source` solo para comprender el contrato operacional;
- resolver una segunda vez referencias que ya fueron publicadas resueltas.

## 6. Runtime boundary

Runtime consume `runtime.json`.

Runtime es dueño de:

- evaluación;
- lifecycle;
- priority;
- management/deactivation runtime;
- adoption/effective configuration;
- hechos durables de ejecución.

Runtime no debe conocer layout ni consumidores Web.

## 7. Delivery boundary

Delivery consume:

- hechos producidos por Runtime;
- `delivery.json` exacto de la misma configuración efectiva.

Delivery puede conocer direccionamiento semántico mediante keys estables:

- `tool_key`;
- component keys;
- subcomponent keys;
- mensajes/capabilities;
- visual targets;
- color semántico.

Delivery no debe conocer geometría del layout ni estilos CSS.

## 8. Live Projection

Live Projection representa el estado operacional actual ya resuelto por el Engine.

Debe preservar los invariantes registrados:

- priority se resuelve antes de publicar;
- eclipsed Rules no se convierten en alarmas actuales predominantes;
- managed/deactivated siguen siendo dimensiones del estado actual cuando la condición física continúa activa;
- Web no re-resuelve prioridad, Messages ni reglas de negocio.

## 9. Diferencia KPI vs Alarm en Web

### KPI

KPI Delivery puede distribuir valores por component/destination y el Collector mantiene stores por componente.

### Alarm

Alarmas necesitan una vista global porque la Web construye un layout completo.

```text
Live Alarm Projection
        +
ToolStructure local de la Web
        ↓
Alarm layout resolver
        ↓
store/modelo global de alarmas
        ↓
layout completo
```

La Web usa las keys entregadas por Alarm Delivery para localizar components/subcomponents dentro de su propia `ToolStructure`.

La Web decide:

- posición concreta;
- estructura visual;
- composición del layout;
- CSS/theme;
- cómo representar el color semántico.

El Engine decide:

- qué alarma existe;
- qué estado operacional tiene;
- qué color semántico corresponde;
- a qué Tool/component/subcomponent lógico apunta.

## 10. Analytics

Fuentes útiles para History/Analytics:

- Occurrence/Episode;
- Journey;
- Evidence;
- management/deactivation;
- routing;
- priority transitions;
- configuration revisions;
- delivery/publication revisions cuando sean relevantes para trazabilidad.

Mantener separadas:

- Live Projection;
- Management Projection;
- History/Analytics.

Web no lee WAL directo.

Analytics no modifica estado del Engine.

## 11. Dependencias invertidas actuales

`VERIFIED / CURRENT IMPLEMENTATION / TO REMOVE`

La implementación actual contiene dependencias desde backend Alarm Materialization hacia paquetes bajo `web`, incluyendo configuración/proyección y contratos de Tools.

Estas dependencias deben tratarse como deuda de frontera, no como contrato a preservar.

No mover esos módulos a otro ownership solo para mantener el acoplamiento. Primero determinar si la dependencia debe existir. Para el flujo propuesto de Materialization, la expectativa es eliminarla y consumir únicamente el contrato operacional publicado.

## 12. Conflicto con decisiones registradas

`CONFLICT`

R3.6M-006B.2 registrada asigna a Materialization adquisición del candidato, lectura de Alarm revision + Confirmed Tool Catalog y resolución B.2.

La frontera refinada en este Project propone:

```text
Command Center Resolution
        ↓
Resolved Alarm Configuration
        ↓
Engine Materialization
```

Por tanto, la decisión previa debe ser reemplazada o refinada explícitamente en `atlanticus-decisions` antes de considerar esta frontera DESIGN FROZEN.

## 13. OPEN

1. Definir el schema exacto de `Resolved Alarm Configuration` publicado en Cosmos.
2. Definir qué campos pertenecen exclusivamente a `runtime.json`.
3. Definir qué campos pertenecen exclusivamente a `delivery.json`.
4. Determinar si `tool_kind` tiene alguna semántica Engine real después de resolución.
5. Eliminar dependencias backend → Web una vez congelado el contrato.
6. Reconciliar B.2 en `atlanticus-decisions`.
7. Solo después, evaluar extracción física a un scope `ada-alarm-engine`.
8. KPI Engine se revisará en un hito separado; no mezclarlo con esta corrección.
