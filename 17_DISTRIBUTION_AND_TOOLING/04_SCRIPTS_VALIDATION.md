# Scripts and Integrity Validation

Estado: **CURRENT DIRECTION**

## Filosofía

Cada frontera importante mantiene su script de validación propio.

Ejemplos de categorías ya existentes:

```text
scripts/backend
scripts/connectivity
scripts/deployment
scripts/integrations
scripts/scopes
scripts/web
```

La raíz actual ya contiene check scripts para Backend y Web.

## Pendiente

Continuar incorporando validadores por componente/capability a medida que se cierran.

Cada check debe proteger contratos reales:

- imports;
- Ruff/format;
- tests relevantes;
- build/package;
- locks;
- mirrors;
- distribution contract cuando aplique.

No agregar tests CSS o de existencia interna como sustituto de integridad.

## Master Integrity Gate

Crear un gate maestro:

```text
scripts/check.sh
scripts/check.bat
```

o equivalente final consensuado.

Responsabilidad:

```text
master
├── backend
├── connectivity
├── integrations
├── scopes
├── web
├── deployment/distribution
└── repository consistency
```

El master no reimplementa validadores.

Sólo:

- descubre/invoca gates;
- agrega resultado;
- falla si cualquier frontera falla.

## Local process scripts

`local-process.sh` se mantiene como herramienta específica de artifacts/processes.

No convertirlo en master gate de todo el repositorio.
