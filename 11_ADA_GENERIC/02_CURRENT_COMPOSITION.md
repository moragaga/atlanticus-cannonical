# ADA Generic — Current Composition

Estado: **CURRENT / CORE CLOSED HISTORICALLY / GENERATED STARTER MANAGER-NAVIGATION GAP OPEN**

Implementación inspeccionada: `moragaga/atlanticus@c2bf25e353b890dc8fd8553ad375745d23ec7154`. El cierre Web no equivale a requalification integral de ADA Generic.

## Composición y bootstrap CURRENT

ADA Generic integra branding, operational header y shell, Navigation, alarm surfaces, content state, render binding, operational state, runtime experience, global indicators, time status y consumos configurados. Capabilities conservan ownership independiente; Atlanticus core no depende de ADA.

```text
AdaGenericSettings
→ Tool persistence settings
→ optional Storage / Tool Projection Cosmos clients
→ ToolPersistenceComposition
→ resolve_operational_tool_projection()
→ READY | UNCONFIGURED | UNAVAILABLE | INVALID
```

Un estado distinto de READY preserva la Web base y sus diagnósticos. Runtime lee Tool Projection durable, no Source ni fallback implícito. Una Projection Tool válida no requiere Source disponible. Cuando Tool READY y KPI Delivery Cosmos está configurado, se compone el Collector existente; el polling es lazy/worker-local y browser consume caché, no Cosmos inline.

`OperationalRenderBinding` comunica sólo estructura. Collector y render mantienen dependencias separadas. `1 ToolComponent → 1 dcc.Store KPI`; Subcomponents no generan stores adicionales. El body concreto pertenece al consumidor de la Tool.

## Manager y Navigation CURRENT en el core

- La composición operacional monta Navigation y autorización sin exigir Identity para la Home pública.
- Manager se integra sólo cuando hay dependencies/stores; el bootstrap puede incorporar identidad local explícita conforme al entorno. `ADA_MANAGER_PERSISTENCE_PROVIDER`: `auto|local|durable|disabled`.
- En CLI, `auto+local → local`; `auto+production → disabled`; `local` sólo en local; `durable` requiere stores y en producción un IdentityProvider externo real (CLI durable actual sólo local). No simular la identidad productiva mediante LocalIdentityProvider.
- Navigation lee la proyección compartida con Manager donde se integra, y no inventa un menú fijo; Home es accesible con Navigation vacía.
- Manager Home `/manager`, header y sidebar son propios y obtienen módulos del `ManagerModuleRegistry`; Navigation operacional y Manager authorization son fronteras distintas.
- Root autorizado/local confiable pueden recibir `administrative_override` conforme al contrato, no existe override automático para invitados o usuarios sin privilegios.

## Composition externa del Starter CURRENT

`tooling/distribution/web/starter/ada/src/application/composition.py` implementa `create_composition(binding)`, deriva `create_local_operational_composition()` y añade un módulo Web de ejemplo. `application/runtime.py` llama al host existente `run_operational_application(composition_factory=...)`; no hay segundo bootstrap ni segundo Manager.

**Brecha explícita:** `qualify_starter.py` inyecta `ADA_MANAGER_PERSISTENCE_PROVIDER=disabled`. El Docker ADA probado también se lanzó con esa configuración. Aunque el Manager/Navigation original existe en el core, esas pruebas no verifican su acceso, Home/sidebar/header, configuración/publicación/proyección de Navigation ni visual real en el Starter.

**Finding comprobado manualmente:** el contenedor ADA devolvió HTML `Acceso denegado` para `/example`, pese a que la probe sintética registró `PASS`. El middleware sólo autoriza solicitudes de documentos HTML a rutas permitidas por Navigation; registrar la página Dash no autoriza automáticamente su visita. El motivo exacto se verificará con requests HTML/status y una proyección controlada, no eliminando el middleware.

## Evidencia y límites

`SOURCE_SMOKE / PASS` y `PORTABLE / PASS` de Generic/ADA bajo Python 3.14.2, y builds/liveness Docker de ambos, fueron reportados por el usuario. Ninguna de esas verificaciones demostró Manager administrativo visible ni navegación ADA de browser. La implementación vigente de `APPLICATION_PUBLICATIONS_ROOT` permite publicar assets fuera de la instalación.

## Siguiente foco único propuesto

`WEB-STARTER-MANAGER-NAVIGATION-VISUAL-INTEGRATION-QUALIFICATION`: primero diseño y acuerdos sobre composición de Manager y principal local del perfil correspondiente; después pruebas de ruta `/manager`, publicación/proyección de `/example`, solicitudes browser HTML autorizadas/denegadas y comprobación visual de header Manager, navegación y CSS. No fusionar headers, no añadir Manager ADA al Starter Generic por defecto, no bypass de identidad. Docker Gunicorn/8000, plantillas y emuladores son trabajos posteriores separados.
