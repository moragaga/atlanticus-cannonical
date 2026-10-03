# Alarm Engine — Qualification Baseline

Estado: **CURRENT — ada-contracts/Command Center cutover gate IN PROGRESS; blocked outside Alarm Engine behavior**.

## Authority

```text
implementation: atlanticus@6725237a19c4442fdfa1b32c3410c124e9348dbc
canonical base: atlanticus-cannonical@9a6dafce3382d2d21fa0ebf790af57daf8715e7a
Python: 3.14.2
```

Los resultados siguientes son logs locales aportados por el usuario y lectura de Git. No equivalen a CI remota.

## Qualification observada en el cutover

| Package / scope | Evidencia observada |
|---|---|
| `ada-contracts-alarms` | 51 tests PASS; Ruff GREEN; build GREEN; schemas incluidos en wheel. |
| `ada-contracts-tools` | 10 tests PASS; Ruff GREEN; build GREEN. |
| `ada-command-center/domain/alarms` | 6 tests PASS; Ruff GREEN; build GREEN; reducido a source identity + routing policy. |
| backend aggregate | `package=false`; lock/sync GREEN; build correctamente omitido. |
| `backend/alarms/core` | 174 tests PASS; Ruff GREEN; build GREEN. |
| `backend/alarms/materialization` | 64 tests PASS; Ruff GREEN; build GREEN. |
| `backend/alarms/persistence` | 89 tests PASS; Ruff GREEN; build GREEN. |
| `processes/alarms-delivery` | 29 tests PASS; Ruff GREEN después de declarar deps de integración sólo en dev; qualifier continuó más allá. |
| `processes/alarms-materialization` | 43 tests PASS; Ruff corregido y qualifier continuó. |
| `processes/alarms-runtime` | 145 tests PASS; Ruff GREEN; build GREEN después de remover tests estructurales/restrictivos. |
| `web/alarms/configuration` | 123 tests PASS; Ruff GREEN; build GREEN. |

## Delivery integration dependency — CLOSED

`tests/test_engine_delivery_integration.py` ejecuta Runtime real y usa pandas para construir datos de prueba.

Por eso Delivery declara únicamente en `dev`:

```text
pandas==3.0.3
ada-command-center-alarms-runtime-process==1.0.0
```

No se convirtió Runtime ni pandas en dependencia productiva de Delivery.

## Testing cleanup — CURRENT POLICY APPLIED

Se eliminaron tests que congelaban:

- mirrors comentados;
- `__version__`;
- metadata exacta de packaging;
- lista/orden exactos de dependencies;
- exports/imports como arquitectura;
- presencia/ausencia de símbolos;
- contenido físico exacto del wheel;
- estructura interna del package.

Se conservan tests de comportamiento, invariantes, recovery, integración y contratos de datos con significado funcional.

## Gate actual — BLOCKED

El qualifier llegó a:

```text
scopes/ada-command-center/web/application/ada-command-center-configuration-manager
```

y falló durante collection porque Command Center todavía consume API superseded de Users:

```text
UsersAdministrationStore
UserRecord
CosmosUsersStore
promoted=
```

La capability genérica CURRENT usa:

```text
UsersRegistryStore
ToolMembershipStore
UsersRuntimeStore
UsersAdministrationService(... memberships=...)
```

Este bloqueo es drift de Command Center frente al nuevo baseline Web; no es evidencia de regresión del contrato Alarm.

## Próxima secuencia congelada

```text
1. Command Center parity con ADA para Users / Profiles / Navigation / Manager
2. qualification focal de esa paridad
3. retomar el mismo qualifier ada-contracts
4. terminar gate completo
5. inspeccionar diff/dependencies/distribution
```

## UNVERIFIED

- CI limpio del SHA actual;
- Docker/Azure;
- packaging distribuido completo después del cierre del gate;
- arranque independiente Engine/Delivery desde distribución final;
- recursos Azure reales;
- smoke durable de Command Center sobre el nuevo wiring.
