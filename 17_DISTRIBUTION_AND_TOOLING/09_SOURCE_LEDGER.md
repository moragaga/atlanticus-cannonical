# Distribution and Tooling — Source Ledger

Estado: **AUDIT LEDGER**

## Current Git

Corte:
`moragaga/atlanticus@685924322c9cc0d625d112e25297a407f7a46acb`

Verificado:

- `scripts/backend/{check.py,check.sh,check.bat}`
- `scripts/web/{check.py,check.sh,check.bat}`
- `scripts/local-process.sh`
- `deployment/processes/bundle.py`
- `deployment/processes/Dockerfile`

## Historical flow recovered

Se preserva como dirección previa:

```text
Source process
→ Artifact
→ Distribution
```

y el concepto de generar primero un proceso de qualification/test desde el mismo contrato que luego materializa el proceso final.

También se preserva el objetivo de generar ADA Generic como aplicación distribuible.

Cualquier tooling histórico no presente en `main` debe considerarse referencia hasta ser reconciliado, no implementation current.
