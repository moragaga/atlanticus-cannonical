# ADA Web — Shell Boundaries

Estado: **CURRENT CONTRACT / LOCAL MANAGER HEADER CLOSED / STARTER VISUAL QUALIFICATION OPEN**

Inspección estática del nuevo cierre: `moragaga/atlanticus@392ee281a32396516fb08c23c63514d8cbdb3489`. La calificación visual local fue confirmada por el usuario, no por una ejecución del asistente.

## ADA Operational Shell

`ada-generic-application` compone header y shell operacional ADA; posee identidad contextual, Navigation operacional, KPI/Alarm/Tool UI y estado de operación. Navigation representa avatar e insignia conforme al principal provisto. Para usuarios locales reconocidos Jane/John, el binding de ADA respeta la paleta original de Users para **ambos elementos**. La selección aleatoria ocurre por inicio de proceso si no se fija `ATLANTICUS_LOCAL_IDENTITY_SUBJECT_ID`.

## Atlanticus Manager Shell

Manager sigue siendo capability genérica de Atlanticus, independiente de ADA. Posee `/manager` como Home, header, sidebar administrativo, `ManagerModuleRegistry`, workflow y permisos propios. La composición ADA conecta Manager con los módulos administrativos, incluida Navigation Configuration.

La decisión `manager_decisions/ATLANTICUS_MANAGER_GLOBAL_RULES_2026-09-02.md` permanece **APROBADA / CONGELADA**: mismo registry filtrado por autorización para Home/sidebar, cards sin workflow y separación de dominios. La personalización del header por marks es opcional; el core no carga logos ADA por defecto.

**Cierre visual del core local:** ADA configura marcas ADA + Atlanticus (sin Los Pelambres en este header), `Gestor de configuración ADA`, contexto de sección, enlaces `Manager Home`/`Volver a la aplicación` según host y etiqueta `Usuarios`. Se retiró **sólo** la visualización del nombre de usuario en el header; `ManagerPrincipal` y autorización permanecen.

## Regla frozen

No fusionar header/sidebar Manager con header/Navigation operacional ADA ni con la superficie de bootstrap/readiness. Compartir estilos o primitives no transfiere identidad privilegiada ni ownership. La navegación operativa publicada/autorizada sigue siendo fuente de rutas cuando aplica.

## Brecha del Starter distribuido

La ejecución histórica del Generic/ADA Starter y del contenedor ADA fijó `ADA_MANAGER_PERSISTENCE_PROVIDER=disabled`, por lo que no cualificó el Manager visible **desde el Starter**. La prueba de `/example` en Docker devolvió HTML `Acceso denegado`, mientras la probe HTTP sintética no imitaba completamente `Accept: text/html` de navegador. Este finding permanece **OPEN** aunque el core ADA Generic local tenga el header y los colores aceptados.

No agregar menú hardcodeado, root automático, segundo bootstrap, segundo Manager ni bypass al resolverlo. Docker Gunicorn/8000, env/secretos, emuladores y despliegue real constituyen fronteras posteriores distintas.
