# ADA Command Center — Web Application

Estado: **CURRENT DIRECTION / CONFIGURATION CAPABILITY IMPLEMENTED / APPLICATION SHELL PENDING**

Command Center requiere Web propia.

No se integra como página de `ada-generic-application`.

## Implementación CURRENT

Ya existe una capability Web de configuración de alarmas bajo:

```text
scopes/ada-command-center/web/alarms/configuration
```

Esta capability posee:

- contrato Alarm Configuration;
- Source/Release;
- Projection base;
- integración Manager;
- workspace/validation/history;
- superficie Web capability-local en modo documental.

Esto **no** equivale todavía a la aplicación Web completa de Command Center.

Permanece pendiente:

- package/application entrypoint propio;
- shell/header final;
- navegación general de Command Center;
- montaje de Dashboard e Historia/Explorer;
- montaje final de las capabilities Manager dentro de ese shell;
- provider/runtime composition productiva.

## Shell

Tendrá shell/header/navegación propios de Command Center.

No reutilizar automáticamente:

- header operacional ADA;
- header Manager ADA.

Sí puede reutilizar:

- primitives;
- tokens;
- branding;
- capabilities Atlanticus;
- `atlanticus.web.manager` para las superficies administrativas que tengan Source/Projection.

El diseño exacto del header queda pendiente.

## Identidad

Producción utiliza Microsoft Entra ID.

Reutilizar la capability transversal de identidad; no crear un segundo sistema de autenticación.

## Superficies iniciales candidatas

```text
Command Center
├── Dashboard
├── Historia / Explorer
└── Configuración
```

Alarm Configuration ya aporta una capability para la tercera superficie, pero aún no está montada
en una aplicación Command Center final.

## Refresh

No incorporar inicialmente auto-refresh/session-reload como feature heredada de ADA Generic.

Separar:

- refresh de aplicación/sesión;
- actualización de datos Live/Analytics.

La segunda sí puede ser necesaria y debe definirse por contrato.

## User tracking

User Activity es una capability transversal opcional.

No es requisito para que Command Center, Alarm Engine o Analytics puedan funcionar.

Si se integra:

- conserva historia ordenada por página/visita;
- mantiene TTL de 24 h;
- puede enriquecerse con Navigation mediante composition opcional;
- no introduce dependencia desde Alarm Engine ni Analytics hacia User Activity.

Puede quedar fuera del primer Golden Path si no aporta valor directo a esa demostración, sin quedar
excluida de la arquitectura.

## Startup / deployment

Dirección:

```text
Command Center Web
→ resource preparation
→ configuration projection/materialization
→ READY
→ Alarm backend/runtime
```

La Web debe poder mostrar shell/bootstrap/configuración aun sin alarm data o sin Alarm Runtime
desplegado.

Invalid resource contract no se oculta; bloquea la capability dependiente y se muestra en
readiness.
