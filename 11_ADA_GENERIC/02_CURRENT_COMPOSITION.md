# ADA Generic — Current Composition

Estado: **VERIFIED**

Implementación auditada:
`scopes/ada/web/application/ada-generic-application`

## Actualmente compone/consume

Entre otras capacidades:

- branding;
- ADA navigation;
- ADA operational header;
- alarm management summary;
- alarm status;
- content state;
- operational render binding;
- operational state;
- runtime experience;
- source consumption / operational participation;
- time status;
- global indicators;
- session/runtime Web.

## Header

ADA Generic usa el **ADA operational header**.

Esto no se reutiliza como header del Manager.

## Runtime

`create_application_runtime` reúne la composición Web y estados de consumo operacional en un `WebApplicationRuntime`.

## Tests actuales

La suite observada contiene principalmente:
- application/composition contracts;
- operational component materialization;
- operational render binding;
- operational state extraction;
- runtime experience extraction;
- JS smoke de session/wake-lock.

No convertir futuros tests en contratos de CSS.
