# Web Platform — Open Items

Estado: **CURRENT / NEXT QUALIFICATION FROZEN**

Checkpoint: `moragaga/atlanticus@ce1213ec14cdee0be905c042c1cf513d71fb5b2d`.

Los contratos implementados y probados localmente no se reabren para restaurar simetría ni soportar legacy. Mantener foco único y distinguir tests simulados de qualification real.

## Baseline CLOSED / VERIFIED LOCAL / CURRENT

```text
ADA-STORAGE-NAMESPACE
TOOL-PROJECTION-PERSISTENCE
TOOL-PERSISTENCE-RESILIENT-COMPOSITION
ADA-WEB-KPI-COLLECTOR-CAPABILITY
ADA-GENERIC-OPERATIONAL-BOOTSTRAP
ADA-GENERIC-MANAGER-LOCAL-INTEGRATION
ADA-GENERIC-MANAGER-DURABLE-ADAPTER-COMPOSITION
ADA-GENERIC-MANAGER-RESOURCE-CLI
ADA-GENERIC-INTERNAL-COSMOS-CONTAINER-NAMES
```

Verificación local reportada: **157 tests**, Ruff y mirrors aprobados, build wheel aprobado en ADA Generic. No implica validación Docker/Azure.

## Próxima frontera única — PLANNED / NEXT

```text
ADA-GENERIC-DOCKER-REAL-PERSISTENCE-QUALIFICATION
```

Alcance de qualification, **no nuevo rediseño**:

1. Revisar la configuración real del entorno local, sin exponer secretos, y disponer de Blob/Cosmos reales/emulados.
2. Preparar externamente el contenedor Blob actual; comprobar `ada-generic-manager-resources ensure-local` y `validate` contra infraestructura real.
3. Arrancar `ada-generic-application` en modo `durable` local; verificar Web base, Manager y comportamiento no-data.
4. Probar flujo administrativo de Source → publicar → Projection real en dominios del Manager alcanzables por la composición.
5. Reiniciar Web y comprobar persistencia, recuperación y autorización; registrar errores reales sin fallbacks implícitos.
6. Comprobar coherencia de snapshots `AccessRuntime` de identidad y principal del Manager, dado finding estático de dos instancias; no atribuirle impacto no demostrado.
7. Documentar criterios de aceptación y evidencias. Si se revela un defecto, proponer incremento aislado antes de implementarlo.

Límites: este plan no cubre entrega productiva Entra, despliegue Backend, automatización completa de `docker compose up`, provisioning Blob automático ni qualification global de otras aplicaciones.

## Abiertos separados

```text
APPLICATION-RESOURCE-PLAN GLOBAL
PLANNED / OPEN

BLOB-PROVISIONING-PARITY
OPEN

PRODUCTION-IDENTITY-PROVIDER / ENTRA
PLANNED / UNVERIFIED

FULL WEB READINESS / RESOURCE ORCHESTRATION
PLANNED / OPEN

NAVIGATION-MANAGER-AUTHORIZATION-CONSUMER-ALIGNMENT
BLOCKED / SEPARATE

PYTHON-METADATA-ALIGNMENT (baseline 3.14.7 vs ==3.14.2)
PLANNED / SEPARATE

CI REMOTE / FULL RUFF WORKSPACE / FULL PYTEST MONOREPO
UNVERIFIED
```

## Atajos prohibidos

```text
No legacy/shim/alias ni doble contrato.
No hardcode de Tool Configuration.
No nombres de contenedores Cosmos en `.env`.
No Source como requisito de lectura de Projection en runtime.
No fallback silencioso si el provider configurado falla.
No tratar la ausencia de autorización como un acceso permitido.
No convertir el plan parcial Manager en inventario global sin auditoría.
```
