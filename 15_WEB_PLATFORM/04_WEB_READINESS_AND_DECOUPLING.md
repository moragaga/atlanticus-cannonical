# Web Platform — Readiness and Decoupling

Estado: **CURRENT / ADA GENERIC BOOTSTRAP IMPLEMENTED / REAL PROVIDERS UNVERIFIED**

Checkpoint de implementación de este cierre: `moragaga/atlanticus@ce1213ec14cdee0be905c042c1cf513d71fb5b2d`.

## Invariante

La Web debe poder existir con datos, datos parciales, sin configuración/datos persistidos y mientras proveedores opcionales/externos estén indisponibles. No convertir el estado de una capability en fallo global de arranque por comodidad.

```text
WEB PROCESS / BASE COMPOSITION != CAPABILITY READY
```

Para Tool Projection la resolución CURRENT mantiene cuatro estados:

| Estado | Semántica |
|---|---|
| `READY` | Projection válida y utilizable. |
| `UNCONFIGURED` | Provider accesible, sin configuración/proyección vigente. |
| `UNAVAILABLE` | Infraestructura configurada inaccesible. |
| `INVALID` | Contrato/documento no aceptable. |

`UNAVAILABLE` e `INVALID` deben ser diagnosticables. No cambiar automáticamente de provider ni leer legacy si un provider explícito falla.

## Avance ADA Generic — CURRENT

Se reemplazó el arranque anterior basado en la ausencia obligatoria de Tool Source por una composición con `AdaGenericSettings`, `AdaStorageNamespace`, `ToolPersistenceSettings`, `compose_tool_persistence` y resolución de Tool Projection para la Web operacional. El Collector se incorpora cuando la Projection Tool está `READY` y existe configuración Cosmos de KPI Delivery. Los otros estados conservan la definición Web base sin inventar Tool Configuration ni realizar proyecciones a partir de Source al leer en runtime.

**Calificación:** código inspeccionado en el commit indicado y prueba local reportada de ADA Generic con **157 tests aprobados**, Ruff sin errores, mirrors validados y wheel construido. No extender ese resultado a un monorepo completo, CI remoto ni proveedores reales.

## Manager y entorno — CURRENT

`ADA_MANAGER_PERSISTENCE_PROVIDER` selecciona `auto`, `local`, `durable` o `disabled`. `durable` es un **modo de persistencia**, no un periodo de retención ni un tiempo. En este incremento concreto requiere Tool Source `blob` y Tool Projection `cosmos`; reutiliza esas conexiones. Tool Source y Tool Projection **mantienen sus ejes de selección independientes** fuera de ese modo específico del Manager.

El CLI aplica actualmente:

```text
auto + local       → Manager local
auto + production  → Manager disabled
local              → sólo environment local
durable            → Manager Blob/Cosmos, opt-in; CLI sólo local
disabled           → sin Manager; Web operacional independiente
```

Un Manager `durable` productivo requiere `IdentityProvider` productivo inyectado desde el host; el ejecutable CLI no lo inventa. La composición puede construir clientes sin health check de arranque; el preflight es una operación explícita. No equiparar esa propiedad con resiliencia e2e ya demostrada.

**Frontera crítica:** la indisponibilidad de un repositorio de identidad/autorización no permite acceso implícito. La disponibilidad de Web base y la disponibilidad autorizada de Manager deben calificarse por separado.

## No-data y límites

```text
Configuration determines existence/structure.
Data determines state.
Persisted state does not determine Web process existence.
```

Una Tool aún no configurada puede iniciar la Web base. Una Tool configurada sin KPI data conserva estructura con data ausente. Los procesos Backend y los workers Web son independientes; reiniciar Web no debe ser requisito de corrección de un job Backend.

## Finding estático a calificar — OPEN

Inspección del commit: `bootstrap._prepare_manager_identity()` crea un `AccessRuntime` para `ManagerPrincipalBinding`, mientras `create_identity_module()` registra **otro** `AccessRuntime` en `register_services`. Las pruebas locales reportadas no acreditan que ambos mantengan el mismo snapshot bajo solicitudes reales. **VERIFIED STATIC / RUNTIME IMPACT UNVERIFIED**. No declarar coherencia end-to-end de identidad/Manager sin probarla; no introducir un shim ni corregir silenciosamente durante el cierre documental.

## Próxima qualification

Prueba Docker de ADA Generic con Cosmos + Blob reales/emulados, preflight, arranque, operaciones administrativas, publicaciones/proyecciones, reinicio y comprobación de autorización. Esta prueba permanece **PLANNED / UNVERIFIED**. No rediseñar por anticipado otros consumidores ni asumir que `docker compose up` ya satisface todo el flujo.
