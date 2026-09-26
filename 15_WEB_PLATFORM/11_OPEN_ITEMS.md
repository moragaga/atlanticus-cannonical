# Web Platform — Open Items

Estado: **CURRENT / NEXT: STARTER MANAGER–NAVIGATION–VISUAL QUALIFICATION**

Checkpoint Web: `moragaga/atlanticus@c2bf25e353b890dc8fd8553ad375745d23ec7154`.

## CLOSED en el alcance demostrado

- Núcleo ADA Generic y Collector, Manager local/durable y Navigation previa, según sus propios checkpoints históricos.
- Starter Generic y ADA `SOURCE_SMOKE / PASS`, construcción wheelhouse (36 / 108) y `PORTABLE / PASS` offline con Python 3.14.2, reportados por el usuario.
- Docker local Generic/ADA: construcción de ambas imágenes y liveness verificada manualmente; qualification integrada sólo parcial.

## OPEN explícitos

| Item | Estado | Por qué sigue abierto |
|---|---|---|
| Starter Manager Home/header/sidebar | OPEN / NEXT | Pruebas ADA usan `ADA_MANAGER_PERSISTENCE_PROVIDER=disabled`; no hay recorrido de Manager visible desde distribución. |
| Configuración/proyección Navigation y `/example` HTML | OPEN / NEXT | El contenedor ADA mostró Acceso denegado; el probe HTTP no imitó el browser. |
| Estilo completo Atlanticus desde Starter | OPEN / NEXT | Falta qualification visual de Manager/shell/nav/assets integrados. |
| Un Dockerfile local/productivo, Gunicorn, 8000 | PROPOSED / PLANNED | Dockerfile CURRENT es local-only/8050/dev server. |
| Host de identidad productivo / Entra | PLANNED / UNVERIFIED | Identidad local no representa producción. |
| Plantillas `secrets.json`, `dev/uat/prd.mapping-env.csv` | PLANNED | Deben ser inactivas, sin valores sensibles y selección explícita. |
| Cosmos/Azurite Docker + provisioning/restart | PLANNED / UNVERIFIED | No hay prueba de los proveedores durables en este frente. |
| Global resource plan, primer Tool real | OPEN / OTHER SCOPE | No pertenece al cierre de Starter. |
| Doble AccessRuntime Identity/Manager histórico | UNVERIFIED | Revalidar con integración si la evidencia lo exige, no inventar defecto. |
| Python 3.14.7 | PLANNED / DEFERRED | 3.14.2 permanece CURRENT por decisión del usuario. |
| Azure, CI remoto, full Ruff/pytest | UNVERIFIED | Sin evidencia sobre esos entornos. |

## Próximo foco único

`WEB-STARTER-MANAGER-NAVIGATION-VISUAL-INTEGRATION-QUALIFICATION`. Debatir primero la composición explícita de Manager en el perfil correspondiente y la autorización local legítima; diseñar el recorrido de publicación/proyección Navigation para `/example`, requests reales `Accept: text/html` (permitido y 403), Manager Home/sidebar/header y revisión visual del estilo. Solo después de consenso implementar un incremento pequeño verificable, con espejo comentado. Sin nuevo framework, sin bypass, sin mezclar shells ni acoplar Atlanticus Generic a ADA.

La antigua etiqueta de Navigation core BLOCKED no se reabre: el finding pertenece al **consumidor Starter distribuido**, no demuestra ausencia del capability core.
