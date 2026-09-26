# ADA Web — Shell Boundaries

Estado: **CURRENT CONTRACT / GENERATED STARTER VISUAL QUALIFICATION OPEN**

## ADA Operational Shell

`ada-generic-application` compone el shell y header operacional ADA. Posee identidad contextual, Navigation operacional, KPI/Alarm/Tool UI y presentación de estado de operación.

## Atlanticus Manager Shell

Manager es una capability genérica reusable de Atlanticus y no depende de ADA. El Manager ADA construye módulos de configuración (incluida Navigation) sobre el Manager genérico. Manager posee Home `/manager`, header, navegación/sidebar administrativo, `ManagerModuleRegistry`, estados/workflow y permisos propios. Según `manager_decisions/ATLANTICUS_MANAGER_GLOBAL_RULES_2026-09-02.md` (APROBADO / CONGELADO), la Home y el sidebar consumen el mismo registry filtrado por permisos antes del render. Cards no contienen acciones workflow y el sidebar no es una réplica de Navigation operacional.

## Regla frozen

No fusionar:
- header y sidebar Manager;
- header y Navigation operacional ADA;
- consola mínima previa de login/bootstrap/readiness.

Pueden compartir tokens, CSS, branding y primitivas genéricas donde corresponda, pero nunca identidad privilegiada ni ownership de capacidades. La Navigation visible de una aplicación debe venir de la configuración proyectada/autorizada cuando ese contrato aplica.

## Brecha en el Starter de este cierre

Generic Starter es deliberadamente mínimo y ADA-independent. El overlay ADA usa el shell operacional existente, pero el probe y el contenedor ADA de este hito se lanzaron con `ADA_MANAGER_PERSISTENCE_PROVIDER=disabled`. Por ello no se verificó Manager Home/header/sidebar ni el flujo para administrar y proyectar Navigation. El usuario reportó que esto impide navegar y observar la apariencia completa de Atlanticus.

En Docker ADA, `/example` devolvió HTML `Acceso denegado`. El middleware de Navigation niega solicitudes HTML para rutas fuera de la definición permitida (excepto override legítimo). `SOURCE_SMOKE`/`PORTABLE` emplearon un probe HTTP insuficiente para verificar la misma cabecera `Accept: text/html` de navegador. No afirmar que el componente Manager haya sido eliminado del core: lo que falta es el recorrido **integrado y cualificado** desde la distribución.

## Próximo foco — PROPOSED

Debatir y definir cómo habilitar Manager desde el Starter de perfil apropiado, sin contaminar Generic con ADA; configurar/publicar/proyectar la ruta de ejemplo por el mecanismo actual de Navigation; probar HTTP HTML 200/403 esperado y revisar visualmente header administrativo, sidebar, Navigation y assets. No crear menú hardcodeado, identidad root automática ni bypass, y no ampliar este incremento a Docker productivo/secretos/infraestructura.
