# Web Platform — Readiness and Decoupling

Estado: **CURRENT DIRECTION**

## Invariante

La Web debe poder **existir**:

- sin datos;
- sin backend jobs;
- sin Cosmos configurado cuando sea opcional;
- sin Storage configurado cuando sea opcional;
- mientras una dependencia operacional está iniciando.

Esto prueba desacoplamiento real.

## Separar disponibilidad y readiness

```text
HTTP / SHELL AVAILABLE
        ≠
APPLICATION READY
```

Modelo candidato:

```text
Web process
    ↓
PUBLIC/SYSTEM SURFACE AVAILABLE
    ↓
Resource + Projection readiness
    ├── READY
    ├── DEGRADED
    └── ERROR
```

### READY

Todos los requisitos obligatorios están preparados y las proyecciones mínimas están disponibles.

### DEGRADED

La Web funciona, pero falta información o una integración opcional/no disponible.

Ejemplos:

- Backend aún no desplegado;
- Cosmos opcional ausente;
- Storage opcional ausente;
- no existen datos todavía.

### ERROR

Existe una configuración/contrato obligatorio inválido.

Ejemplos:

- partition key incorrecta;
- TTL incompatible;
- source malformado;
- proyección requerida inválida.

La Web no debe fingir disponibilidad funcional de esa capability.

## Front sin datos

Las vistas deben representar explícitamente estados:

```text
READY
STALE
SOURCE_ERROR
CONSTRUCTION / NO DATA
```

según el contrato correspondiente.

No usar crash/startup failure de toda la Web como sustituto de estado funcional cuando la shell puede explicar el problema.

## Backend independence

Después del bootstrap:

```text
Backend job
→ conecta a recursos conocidos
→ procesa
```

No depende de callbacks, memoria ni lifecycle del Web worker.

Reiniciar Web no debe convertirse en requisito para que un job siga siendo correcto.
