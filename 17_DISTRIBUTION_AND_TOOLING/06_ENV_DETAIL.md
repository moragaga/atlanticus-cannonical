# env.detail Contract

Estado: **CURRENT DIRECTION / MASTER VARIABLE IMPLEMENTADA; OTROS ARCHIVOS env.detail NO AUDITADOS GLOBALMENTE**  
Master contrastado en `moragaga/atlanticus@94f26213ca28b550baf53d8ee34e34da7538ad17`. La política documental general de `env.detail` no se convierte en certificación de todos los proyectos.

## Problema actual

`env.detail` existe, pero muchos archivos contienen únicamente:

```text
KEY=value
```

sin explicar qué significa, por qué existe, valores permitidos, si es sensible o quién lo consume. Esto continúa como deuda documental de productización fuera de Master.

## Objetivo

Cada proceso/aplicación estable debe tener un `env.detail` verdaderamente documental. Debe permitir entender cada variable sin leer el código.

## Información mínima por variable

Para cada entrada documentar:

- `key`;
- `purpose`;
- `required` / `optional`;
- `accepted values` / formato;
- ejemplo no sensible;
- `sensitive` yes/no;
- fuente esperada;
- razón arquitectónica;
- `owner/consumer`.

El formato final puede seguir siendo sencillo y humano; no convertirlo en un schema innecesariamente complejo.

## Producción

`env.detail` **NO** contiene secretos. Es referencia/contrato. El runtime productivo continúa usando mecanismos acordados de configuración y resolución de secretos. No incluir un ZIP protegido real, contraseña, token o credencial en el repositorio, Starter ni archivo de ejemplo.

## Ejemplo conceptual histórico

```text
PI_SOURCE=NOTPII
# purpose: selects PI source provider
# accepted: NOTPII | PI_WEB_API
# required: yes
# sensitive: no
# reason: runtime must select exactly one PI adapter
```

El ejemplo expresa la política documental, **no** una nueva configuración aprobada de Master ni una auditoría de PI actual.

## Variable Master realmente implementada

```text
ADA_MASTER_PROJECTION_MATERIAL_PATH
```

| Propiedad | Contrato actual |
|---|---|
| Owner | ADA Generic/Starter; lector `StarterMasterMaterialReader` |
| Propósito | Ruta de archivo al material Master protegido externo |
| Requerida | **No**: ausencia produce página informativa sin login |
| Formato | Ruta absoluta a un ZIP Master; externa al proyecto distribuido |
| Sensible | La **ruta** no es contraseña, pero debe administrarse cuidadosamente; el archivo apuntado es material de acceso sensible |
| Valor por defecto conceptual | Vacío/ausente, no crear cuenta Master local implícita |
| Validación runtime | `ABSENT`, `PRESENT` e `INVALID`; binding de identidad al namespace/ambiente al autenticar |
| Consumers | `AdaGenericSettings` y Starter `application.runtime` |

No agregar variables distintas para Master por conveniencia, ni utilizar esta ruta como prueba de warmup/upload/Key Vault productivo ya implementado. El archivo real se genera mediante el tooling ADA y permanece fuera de la distribución.

## Evidencia y límites

En 001C se observó distribución smoke `BUILT_UNQUALIFIED` de 67 wheels y HTTP 200 con la variable ausente y presente. El usuario informó login real tras generar material nuevo. La validación de toda la matriz `.env.detail` (incluido `kpi-runtime`) y el mapping DEV/UAT/PRD siguen **OPEN / OTHER FOCUS**; no certificar variables de otros procesos a partir de Master.
