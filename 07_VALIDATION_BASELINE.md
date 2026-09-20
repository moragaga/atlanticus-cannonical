# Atlanticus — Validation Baseline

Estado: **CURRENT**

## Autoridad de implementación

```text
moragaga/atlanticus@d71e94d12fa31a986b3ecc0262fbbb6ef2e4a3dd
```

Parent:

```text
107c7570061e0d31828b1d3e9b9fc6336a698809
```

Tree:

```text
41c299861d14a9691cbd3461dbca8bb466dfc156
```

## Regla

Qualification y tests son evidencia de propiedades del contrato CURRENT.

No son autoridad para conservar contratos, schemas, adapters o aliases SUPERSEDED.

No inventar un PASS no observado.

## KPI Registry cutover — evidencia observada

```text
Registry Core                  6 PASS
Registry Configuration       30 PASS
Registry Projection Local     2 PASS
Registry Projection Cosmos    3 PASS
Definition alignment         35 PASS
Configuration Manager        32 PASS

TOTAL
108 PASS
```

Además se verificó:

```text
legacy symbol scan
clean en alcance migrado

Registry CSS
byte-identical

Registry css.list
byte-identical

Registry IDs
byte-identical
```

## KPI Definition cutover — evidencia observada

```text
Definition Core              14 PASS
Definition Configuration     23 PASS
Definition Projection Local   3 PASS
Definition Projection Cosmos  4 PASS
Configuration Manager        32 PASS

TOTAL
76 PASS
```

Además se verificó:

```text
legacy monolithic import scan
clean en Definition + Manager

Definition CSS
byte-identical

Definition css.list
byte-identical

Definition IDs
byte-identical

git diff --check
PASS
```

## Política de tests Web

Probar:

```text
behavior
contracts
invariants
regressions
critical flows
```

No crear tests cuyo único objetivo sea:

```text
CSS visual
responsive
spacing
branding
apariencia
estructura visual
existencia/no existencia de funciones o clases
implementación accidental
```

Los mirrors pedagógicos existentes pueden verificar equivalencia cuando esa entrega es una regla
explícita del módulo.

## Python metadata

Baseline Project:

```text
Python 3.14.7
```

Packages KPI CURRENT observados:

```text
requires-python ==3.14.2
```

Estado:

```text
PYTHON-METADATA-ALIGNMENT
OPEN / SEPARATE
```

## UNVERIFIED

```text
remote CI
full monorepo pytest
full workspace Ruff
targeted Ruff de los dos cutovers KPI
python:3.14.7-slim-trixie global qualification
Azure productive wiring de nuevos resources Cosmos KPI
```

Git continúa SOLO LECTURA para el asistente.
