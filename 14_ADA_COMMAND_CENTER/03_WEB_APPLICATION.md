# ADA Command Center — Web Application

Estado: **CURRENT — capability Alarm Configuration, Web Tool Catalog/Discovery/UI y host temporal; C1 ownership CLOSED; C2 Source Key compartida CLOSED; Starter distribuible PLANNED**. Implementación comprobada: `atlanticus:main@18029e19ff01e58b9c9399c132ff32b5ca913f06`.

Command Center necesita su Web propia; no se monta automáticamente como página de `ada-generic-application`.

## Capabilities actuales

```text
scopes/ada-command-center/web/alarms/configuration
scopes/ada-command-center/web/alarms/persistence
scopes/ada-command-center/web/alarms/projection-local
scopes/ada-command-center/web/alarms/projection-cosmos
scopes/ada-command-center/web/tools/catalog
scopes/ada-command-center/web/tools/discovery-cosmos
scopes/ada-command-center/web/tools/catalog-manager
scopes/ada-command-center/web/application/ada-command-center-configuration-manager
```

C1 trasladó Catalog/Discovery server-side exclusivamente Web a `web/tools/`; `web/tools/catalog-manager` posee UI y callbacks propios. `backend/tools` está SUPERSEDED; `domain/tools` preserva manifest transversal. El host temporal integra estas bibliotecas, sin duplicar su implementación ni convertirse en Starter.

## Host temporal y configuración durable

`ada-command-center-configuration-manager` monta Alarm Configuration y Tool Catalog bajo la superficie `/manager`, con Tool Catalog integrado como Manager Entry `/tool-catalog`. El host posee composición, principal/autorización, elección de providers y clientes externos. Local usa Source/Projection local cuando se configura; durable usa Alarm Source Blob y Alarm Projection Cosmos; la existencia de adaptadores no certifica recursos físicos.

**Delta C2:** el host importa `ALARM_CONFIGURATION_SOURCE_KEY` como texto desde `ada_command_center.domain.alarms` y construye `atlanticus.web.source.models.SourceKey` sólo en la frontera Web. La Source Key permanece exactamente `alarm-configuration`; no hay identidad de Source redefinida por host, proceso o `.env`.

La topología física Cosmos se obtiene de `ALARM_CONFIGURATION_PROJECTION_STORAGE_RESOURCE`: nombre `ada-command-center-alarm-configuration-projection`, partición `/partition_key`, override permitido de connection ref. Web resuelve su binding `command-center-cosmos` en composición, no configura el contenedor físico en `.env`. Materialization reutiliza el mismo contrato físico tras C2 pero su conexión de cuenta/base/credencial es ambiental y debe apuntar manualmente al mismo Cosmos. **UNVERIFIED:** enlace físico Web↔Materialization con recursos reales.

El nombre físico del **contenedor Blob sí permanece en `.env` Web**; las conexiones externas Tool Cosmos continúan nombradas y dinámicas. No deducir rutas ni identidades operacionales de productores externos.

## Starter — PLANNED

El Starter propio `ada-command-center-generic` deberá acordar package, entrypoint y composición de las capabilities existentes. No depende automáticamente de ADA Generic ni absorbe Tool Catalog/Domain. Retirar el host temporal sólo cuando un Starter lo reemplace con evidencia de equivalencia. Shell, Dashboard, Historia/Explorer, navegador final y autenticación productiva no están implementados o cualificados por C1/C2.

Producción utiliza Microsoft Entra ID cuando el proyecto integre su identidad; no crear segundo sistema. Navigation/Users/Profiles/User Activity se incorporan solo con contrato y necesidad. No heredar refresh, header o sesiones de ADA Generic por inferencia.

El inicio objetivo permite shell/configuración antes de datos Alarm/Runtime; errores de resource readiness no deben ocultarse. **UNVERIFIED:** ejecución completa de este startup y despliegue durable E2E.

## Shell, identidad y superficies futuras — sin cambios C2

El shell/header/navegación finales son propios de Command Center. Puede reutilizar primitives y `atlanticus.web.manager`, pero no heredar automáticamente el header operacional de ADA ni el header Manager ADA. Superficies iniciales candidatas: Dashboard, Historia/Explorer y Configuración. Alarm Configuration implementa una capability de esta última, no la Web operacional completa.

Separar refresh de aplicación/sesión del refresco de Live/Analytics. No incorporar `auto-refresh` de ADA Generic sin contrato. User Activity es integración transversal opcional y no gate de Engine/Analytics: si se adopta, conserva la semántica previa documentada de visitas por página/TTL 24 h y composición opt-in con Navigation.

```text
Command Center Web
  -> resource preparation
  -> configuration projection/materialization
  -> READY
  -> Alarm backend/runtime
```

La Web debería presentar shell/bootstrap y administración aun cuando no haya datos Alarm o Runtime desplegado; errores de resource readiness deben hacerse visibles y bloquear solo la capability dependiente. Este startup sigue UNVERIFIED físicamente.

## Fronteras

- Web no reevalúa Rules, reordena prioridad ni interpreta `cause_template`.
- El adaptador Cosmos usado por Materialization sigue físicamente bajo `web/alarms/projection-cosmos`: frontera pendiente declarada, no corregida en C2.
- C3, C4, C5, Live/Management/Analytics, UI fin de turno/modal y distribución Docker son frentes independientes.
