# ADA Command Center — Web Application

Estado: **CURRENT DIRECTION / NOT YET IMPLEMENTED**

Command Center requiere Web propia.

No se integra como página de `ada-generic-application`.

## Shell

Tendrá shell/header/navegación propios de Command Center.

No reutilizar automáticamente:
- header operacional ADA;
- header Manager ADA.

Sí puede reutilizar:
- primitives;
- tokens;
- branding;
- capabilities Atlanticus.

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

Puede quedar fuera del primer Golden Path si no aporta valor directo a esa demostración, sin quedar excluida de la arquitectura.

## Startup / deployment

Command Center adopta el mismo patrón Web-first:

```text
Command Center Web
→ resource preparation
→ configuration projection
→ READY
→ Alarm backend/runtime
```

La Web debe poder mostrar shell/bootstrap/configuración aun sin alarm data o sin Alarm Runtime desplegado.

Invalid resource contract no se oculta; bloquea la capability dependiente y se muestra en readiness.
