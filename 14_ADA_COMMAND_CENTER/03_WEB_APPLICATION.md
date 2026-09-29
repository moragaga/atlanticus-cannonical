# ADA Command Center — Web Application

Estado: **CURRENT — Alarm Configuration capability, Tool Catalog/Discovery/UI Web independientes y host temporal Configuration Manager; PLANNED — Starter distribuible propio**. C1 verificado en `atlanticus:main@3961385aecd0eb7e373018fc25e509a71dccc409` (2026-09-29).

Command Center requiere Web propia: no se integra automáticamente como página de `ada-generic-application`.

## Implementación CURRENT — capabilities

```text
scopes/ada-command-center/web/alarms/configuration
scopes/ada-command-center/web/tools/catalog
scopes/ada-command-center/web/tools/discovery-cosmos
scopes/ada-command-center/web/tools/catalog-manager
scopes/ada-command-center/web/application/ada-command-center-configuration-manager
```

`web/alarms/configuration` tiene contrato Alarm Configuration, Source/Release, Projection base, integración Manager, workspace/validation/history y superficie capability-local. No equivale todavía a la Web operacional completa.

**C1 CLOSED estructuralmente:** Tool Catalog consolidación/Blob y Discovery Cosmos se trasladaron desde `backend/tools` a `web/tools`, junto a sus pruebas y espejos. `web/tools/catalog-manager` es una biblioteca UI independiente que expone fábrica de integración Manager: el host temporal la consume en lugar de mantener una copia de su interfaz. Que Catalog y Discovery estén escritos en Python y ejecuten lógica del servidor no implica pertenencia a jobs. `domain/tools` conserva contratos transversales y no absorbe services Web físicos.

La aplicación actual **no** constituye Starter distribuible ni prueba de aceptación visual final de Command Center.

## Host temporal y composición CURRENT

Existe `web/application/ada-command-center-configuration-manager` con entrypoint local, `ManagerSurface`, Alarm Configuration y Tool Catalog integrado como `ManagerEntry(route='/tool-catalog')` dentro de `/manager`. Tool Catalog no es una página Dash autónoma; `pages/manager.py` registra el contenedor `/manager`.

El host compone clientes, principal/autorización, conexiones, providers y stores. `web/tools/catalog-manager` posee UI/callbacks; `web/tools/discovery-cosmos` presta servicio de inspección/confirmación/adopción; `web/tools/catalog` almacena CURRENT en Blob. No hay implementación UI Tool duplicada en el host ni dependencias de la capability UI hacia su `__main__`.

**Configuración física:** el contenedor Blob de Tool Catalog y Alarm Source durable **sí** está en `.env` de Web por decisión explícita. El contenedor Cosmos de Alarm Projection **no** está en `.env` de Web: se deriva de `ALARM_CONFIGURATION_PROJECTION_STORAGE_RESOURCE` con topología `/partition_key`. Las conexiones Cosmos externas Tool continúan nombradas/dinámicas.

## Starter distribuible — PLANNED

Falta `ada-command-center-generic` como package/application entrypoint de Starter propio, que componga inicialmente Tool Catalog y Alarm Configuration. Acordar la ubicación/nombre final antes de implementarlo. No duplicar dominio ni acoplar al Starter de ADA Generic. El host temporal sólo podrá retirarse cuando el Starter cubra y valide sus responsabilidades.

Continúa pendiente: shell/header final, navegación general Command Center, montaje Dashboard e Historia/Explorer, montaje final de Manager capabilities en ese shell y provider/runtime composition productiva. Identidad/usuarios/perfiles/navegación son integraciones opt-in cuando exista demanda y contrato; no convertirlas en dependencias prematuras.

**UNVERIFIED:** build y distribución del Starter, autenticación productiva del nuevo host, UI/browser post-C1, instalación aislada de los dos wheels Web y despliegue Azure de Command Center. C1 sólo ejecutó gates de código/tests/wheels/imports del host en su entorno existente.

## Shell

Tendrá shell/header/navegación propios de Command Center. No reutilizar automáticamente el header operacional ADA ni el header Manager ADA. Puede reutilizar primitives, tokens, branding, capabilities Atlanticus y `atlanticus.web.manager` para superficies Source/Projection. El diseño exacto permanece pendiente.

## Identidad

Producción utilizará Microsoft Entra ID. Reutilizar capability transversal de identidad; no desarrollar segundo sistema de autenticación.

## Superficies iniciales candidatas

```text
Command Center
├── Dashboard
├── Historia / Explorer
└── Configuración
```

Alarm Configuration ya cubre una capability de configuración, pero no está montada en un shell Command Center definitivo.

## Refresh

No introducir auto-refresh/session-reload heredado de ADA Generic por inferencia. Separar refresh de aplicación/sesión de actualización Live/Analytics; la segunda puede requerir contrato propio.

## User tracking

User Activity es capacidad transversal opcional y no condición previa para Command Center, Engine ni Analytics. Si se integra, conserva historia por página/visita, TTL de 24 h y composición optativa con Navigation, sin dependencias desde Engine/Analytics.

## Startup / deployment objetivo

```text
Command Center Web
→ resource preparation
→ configuration projection/materialization
→ READY
→ Alarm backend/runtime
```

La Web debe mostrar shell/bootstrap/configuración sin Alarm data o Runtime desplegado. Un resource contract inválido no se oculta: bloquea la capability dependiente y se refleja en readiness. **C1 no prueba este startup E2E.**

## Regla de ownership C1 y límite pendiente

El código Python server-side exclusivo de Web es Web. `backend` organiza procesos ejecutables y el código que les es específico. `domain` recibe contratos genuinamente compartidos, no infraestructura arbitraria. El job Materialization sigue importando infraestructura de `web/alarms/projection-cosmos` en código actual; registrar esta frontera OPEN sin corregirla incidentalmente durante C2.
