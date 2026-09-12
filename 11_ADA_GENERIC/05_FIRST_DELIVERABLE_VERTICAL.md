# ADA Generic — First Deliverable Vertical

Estado: **PROPOSED / REQUIERE CIERRE DE BOOTSTRAP**

## Objetivo candidato

Conseguir una primera ADA realmente ejecutable/demostrable sobre Atlanticus recorriendo una vertical completa.

No se considera entregable tener paquetes individuales GREEN sin integración usable.

## Vertical candidata

1. Configuration
   - Tool/Structure vigente;
   - KPI/Alarm config requerida;
   - Component/Collector contract.

2. Source
   - Release Model;
   - SourceStore contract;
   - Local semantics;
   - Blob provider cuando el contrato esté GREEN;
   - Source release identificable.

3. Operational Data
   - al menos un Component materializado por su Collector/Producer real;
   - Store vacío y Store con datos son estados válidos.

4. KPI / Alarm
   - integrar sólo lo necesario para esa Tool real;
   - no reabrir los cores ya cerrados sin finding;
   - Alarm Engine conserva qualification R3.5.

5. ADA Generic Web
   - estructura nace de Configuration;
   - datos actualizan estado;
   - alarmas actualizan estado visual;
   - header operacional ADA;
   - navegación/runtime reales.

6. Manager
   - administra configuración mediante su propia aplicación/header;
   - Source/version/conflict observable;
   - no se mezcla con shell operacional.

7. E2E
   - configuración → release → proyección/materialización → consumo → render;
   - flujo verificable en Local primero;
   - Blob después con semántica equivalente.

## Definition of Done candidata

Existe una Tool concreta que puede:
- abrirse en Manager;
- validarse/publicarse;
- producir un release identificable;
- proyectarse/materializarse;
- disponer de al menos un Component con contrato de datos;
- consumir KPI/Alarm donde corresponda;
- arrancar ADA Generic incluso con Store vacío;
- actualizarse cuando llegan datos;
- mostrar estado visual de alarma independiente del dato;
- ejecutarse de punta a punta sin pasos ocultos no documentados.

Este alcance todavía debe calibrarse para escoger la Tool inicial más barata de integrar.
