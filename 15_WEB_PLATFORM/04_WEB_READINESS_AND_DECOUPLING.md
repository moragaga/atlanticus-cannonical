# Web Platform — Readiness and Decoupling

Estado: **CURRENT / REFINED**

## Invariante

La Web debe poder existir:

```text
with data
with partial data
with no persisted business/configuration data
while optional/external providers are unavailable
```

No usar startup failure global como sustituto del estado de una capability.

## Separar existencia y capability state

```text
WEB PROCESS / BASE COMPOSITION
              ≠
CAPABILITY READY
```

Para Tool Projection existe CURRENT:

```text
READY
UNCONFIGURED
UNAVAILABLE
INVALID
```

### READY

Projection válida disponible.

### UNCONFIGURED

Provider accesible pero todavía no existe la configuración/proyección.

### UNAVAILABLE

La operación no puede acceder a su infraestructura.

### INVALID

Se obtuvo un contrato/documento que no puede aceptarse.

Estos estados pertenecen a la capability Tool; no son automáticamente estados del proceso Web.

## Provider composition

Construir clientes/stores puede ser lazy respecto de conectividad.

No ejecutar health checks obligatorios durante composición sólo para demostrar que el provider
está vivo.

La operación concreta debe traducir fallas técnicas al estado/contrato de su capability.

## No-data behavior

```text
Configuration determines existence/structure.
Data determines state.
Persisted state does not determine Web process existence.
```

Una Tool aún no configurada debe permitir probar shell/composición base.

Una Tool configurada sin KPI data debe conservar su estructura y representar data ausente.

## Error visibility

Resiliencia no significa ocultar fallas.

```text
UNAVAILABLE
INVALID
```

deben ser diagnosticables.

No crear fallback silencioso a otro provider ni legacy data cuando un provider explícitamente
configurado falla.

## Backend independence

Backend jobs y Web workers mantienen lifecycle independiente.

Reiniciar Web no debe ser requisito de corrección de un backend job.

## Consumer gap

La infraestructura Tool ya implementa esta separación.

ADA Generic startup todavía debe adoptarla:

```text
ADA-GENERIC-OPERATIONAL-BOOTSTRAP
PLANNED / NEXT
```
