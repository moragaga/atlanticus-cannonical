# ADA Command Center — Product Scope

Estado: **CURRENT DIRECTION**

ADA Command Center es una aplicación ADA independiente orientada a supervisión, configuración y análisis transversal de alarmas.

No es:
- una página de ADA Generic;
- una variante del Manager ADA;
- un dashboard de contadores;
- una UI pegada al motor.

Frontera conceptual:

```text
ADA COMMAND CENTER
├── Web
├── Configuration
├── History / Analytics
└── Alarm Engine
    ├── Core
    ├── Persistence
    └── Runtime
```

## Alarm Engine

El Alarm Engine pertenece funcionalmente a ADA Command Center.

Las Tools ADA consumen resultados operacionales del motor, pero Command Center es owner del dominio de alarmas, su configuración transversal, historia y análisis.

## Finalidad inicial

Command Center debe permitir entender:

- qué ocurrió;
- dónde;
- cuándo;
- cuánto duró;
- qué evidencia existía;
- qué Rule predominó;
- cuáles quedaron eclipsadas/suprimidas;
- qué hizo el operador;
- qué se desactivó/aprobó/rechazó;
- qué reapareció;
- cómo escaló;
- cómo cerró;
- qué patrones se repiten.

El producto debe contar una historia operacional verificable, no sólo mostrar métricas aisladas.
