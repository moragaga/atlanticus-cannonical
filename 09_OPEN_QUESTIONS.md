# Atlanticus — Open Questions

Estado: **CURRENT — EXTENSION RESOURCE INTEGRATION IS THE NEXT SINGLE OPEN FRONT**

## CLOSED

```text
deployment.resources.json contract
consumer ownership of distributed sizing
global 0.5 vCPU / 1.0 GiB default
allowed vCPU/RAM table
GiB -> Docker MiB translation
dynamic Compose override
simulation consumption of the same resource source
pyproject resource authority removal
regeneration preservation of consumer sizing
Docker/local runtime-input contract alignment
process-deployment gate requalification
```

## OPEN / NEXT — Extension integration

El próximo chat debe resolver exclusivamente:

```text
1. Does integrate preserve a custom resource pair for every installed process?
2. Does every newly integrated alias receive exactly 0.5 vCPU / 1.0 GiB?
3. Does integrate reject a malformed or incomplete deployment.resources.json before mutation?
4. Does rollback restore deployment.resources.json if publication fails after that file was replaced?
5. Does integration keep the resource set exactly equal to the final process set?
6. Is a distribution created before this resource frontier explicitly rejected/regenerated rather than supported through a compatibility adapter?
```

La implementación actual ya contiene parte de este comportamiento.

El objetivo siguiente es **qualification and correction only if evidence requires it**, no redesign.

## OPEN / DEFERRED — Distribution qualification

```text
generate every current-head artifact
qualify artifact contents/installability
audit every .env.detail
final distribution regeneration
isolated consumer qualification
real Docker resource-limit smoke
```

## OPEN / SEPARATE

```text
Alarm Runtime migration
Python 3.14.7 / Trixie
production Azure / Entra qualification
```
