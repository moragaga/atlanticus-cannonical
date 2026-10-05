# Atlanticus — Roadmap

Estado: **CURRENT ROADMAP — OPERATIONAL DATA/KPI CLOSED; DISTRIBUTION AND TOOLING NEXT**

## CLOSED / CURRENT relevante

```text
OPERATIONAL-DATA-INPUT-CONTRACT
OPERATIONAL-DATA-LEGACY-CONTRACT-REMOVAL
KPI-RUNTIME-DATA-INPUT-MIGRATION
```

## NEXT único

```text
ATLANTICUS-DISTRIBUTION-AND-TOOLING-FINAL-QUALIFICATION
```

Orden:

```text
1. regenerate current-head artifacts
2. verify every expected artifact is generated
3. qualify artifact contents/installability
4. audit every .env.detail entry
5. classify required/optional/default/system-derived/secret semantics
6. generate distribution
7. run isolated consumer qualification
```

No mezclar Alarm Runtime.

## PLANNED / SEPARATE

```text
Alarm Runtime migration to final Operational Data input contract
Python 3.14.7 / Trixie
production Azure / Entra
Alarm advanced scheduler
History/Analytics and other product work already tracked separately
```

## BLOCKED

```text
Alarm Runtime executable data path
```

Causa: imports del contrato Operational Data removido.

No resolver mediante adapters.
