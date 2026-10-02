# Atlanticus — Current Decisions

Estado: **CURRENT**

## Baseline global

```text
uv, no pip normal
contracts before consumers
backend before frontend
clean root cutover
no legacy adapters/shims/aliases
one focus per increment
Git read-only unless explicit authorization
```

Python target histórico 3.14.7/Trixie permanece diferido; el Web runtime CURRENT de este hito usa 3.14.2.

## Distribution ownership — FROZEN

```text
tooling/distribution/web
= shared engine only
```

Product-specific starter/runtime/distribution behavior pertenece a su scope.

No reintroducir:

```text
tooling/distribution/web/ada
tooling/distribution/web/starter/ada
tooling/distribution/web/starter/command-center
```

## ADA runtime ownership — FROZEN

```text
ada-generic-application
owns ADA runtime lifecycle
owns Master Projection runtime
owns local resource preparation
```

Starter ADA:

```text
may customize host/composition
must not rebuild ADA runtime lifecycle
```

Master Projection es extensión/runtime capability, no aplicación ni distribución tooling.

## Command Center application role — FROZEN

```text
ada-command-center-generic-application
= real product composition root

ada-command-center-configuration-manager
= separate development/testing/qualification application
```

Command Center Starter delega al product root; no recompone la aplicación.

## Wheelhouse artifact policy — FROZEN

```text
compatible SHA256-locked wheel
→ preferred

otherwise SHA256-locked sdist
→ build platform wheel under hash-constrained build dependencies
```

Final wheelhouse contiene wheels, no sdist suelto.

Manifest debe preservar trazabilidad de origen y hash del wheel final.

## Distribution qualification semantics — FROZEN

```text
generic
→ PASS
→ portable runtime probe

ada
→ PRECHECK_PASS
→ image/runtime UNVERIFIED unless qualified separately

command-center
→ PRECHECK_PASS
→ portable artifact/dependency qualification
→ runtime UNVERIFIED
```

No promover `PRECHECK_PASS` a runtime verification.

## Próxima decisión/foco

Acordado como siguiente frente:

```text
ADA + COMMAND CENTER .env.detail
→ configuration contract
→ Storage final/durable + Cosmos local
→ Master Projection contract for both products
```

Después:

```text
lift both applications
→ then Command Center exits scope
→ ADA KPI/data/Collector E2E
→ ADA UI reconstruction
```

## UI authoring — CURRENT fact, not yet distributed env contract

ADA ya implementa:

```text
ContentStatePresentationMode.AUTHORING
```

que suprime overlays degradados visualmente.

Aún no está congelada una variable `.env.detail` que seleccione ese modo.

La propuesta de exponerlo como configuración local-only queda **PROPOSED**, no implementada.

Tampoco existe un estado explícito `NO_DATA`; cualquier contrato nuevo debe decidirse con el flujo Collector/UI real.
