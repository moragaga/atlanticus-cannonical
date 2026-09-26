# Web Platform — Current Gaps

Estado: **CURRENT / WEB STARTER PORTABLE CLOSED / MANAGER STARTER + PRODUCTION OPEN**

Checkpoint de implementación Web inspeccionado: `moragaga/atlanticus@c2bf25e353b890dc8fd8553ad375745d23ec7154`. El resto de capacidades conserva sus checkpoints históricos, sin requalification integral en este hito.

## Core CLOSED en alcance previo

ADA Storage Namespace, Tool Projection Persistence, bootstrap resiliente, Collector runtime wiring, Manager local/durable composition, recursos parciales del Manager, Navigation core/publish/project/consume local. No reabrirlos sin finding y no extender sus GREEN históricos a infraestructura real del Starter.

## Nuevo cierre VERIFIED MANUAL

- Starter editable Generic y overlay ADA, `distribution/` con manifests y Python **3.14.2**.
- SOURCE_SMOKE y PORTABLE PASS en ambos perfiles offline con wheelhouses Generic **36** y ADA **108**; no confundir wheels internos con dependencias externas empaquetadas físicamente dentro de ellos.
- Docker local: imágenes construidas en ambos perfiles y respuesta positiva de `/health/live`. Generic entregó `/example`. ADA soporta `APPLICATION_PUBLICATIONS_ROOT` externo (tests reportados).

## Gaps OPEN / PLANNED

| Elemento | Estado | Evidencia / motivo |
|---|---|---|
| Manager Home/header/sidebar desde Starter | OPEN / NEXT | Qualification y contenedor ADA usan Manager disabled; no se comprobó administración visible. |
| Navigation browser de `/example` | OPEN / FINDING | ADA devolvió Acceso denegado; prueba anterior no reprodujo `Accept: text/html`. |
| Apariencia completa Atlanticus | OPEN | No se probó visualmente Manager, Navigation y operacional integrados desde Starter. |
| Dockerfile unificado local/productivo Gunicorn/8000 | PROPOSED / PLANNED | Docker CURRENT es local-only, 8050 y servidor de desarrollo. |
| Identidad productiva Entra | PLANNED / UNVERIFIED | CLI productivo no debe inventar LocalIdentityProvider. |
| Plantillas inactivas de secretos y mapping DEV/UAT/PRD | PLANNED | Falta definición/validación de selección de plantilla por consumidor; sin datos sensibles en repo. |
| Cosmos/Azurite local, persistencia durable y restart | PLANNED / UNVERIFIED | No probado con emuladores en este frente. |
| Global ApplicationResourcePlan/readiness | OPEN / OTHER SCOPE | Plan del Manager no equivale al inventario global. |
| AccessRuntime Identity/Manager histórico | UNVERIFIED | Observación estática previa de dos instancias; impacto real no probado en Starter. |
| Azure, CI remoto, full monorepo pytest/Ruff | UNVERIFIED | Fuera de evidencia aportada. |

## Desfase documental refinado

Canonical anterior describía toda portabilidad Web como UNVERIFIED; queda **SUPERSEDED sólo para PORTABLE offline probado**, no para Docker productivo ni Manager visual. Python 3.14.7 se difiere como migración futura; 3.14.2 es CURRENT. Manager/Navigation del core existentes no contradicen su ausencia de la qualification: se deshabilitó Manager en ese escenario.

Siguiente foco único **PROPOSED**: `WEB-STARTER-MANAGER-NAVIGATION-VISUAL-INTEGRATION-QUALIFICATION`. Primero diseño/consenso y después implementación aislada. No mezclar Gunicorn, secretos, emuladores, Backend o Command Center.
