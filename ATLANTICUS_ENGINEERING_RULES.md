# Atlanticus — Engineering Rules

## Propósito

Atlanticus es una plataforma modular, liviana y reusable para ADA y otros productos. No convertirla en un framework universal ni en un monolito.

## Autoridad

- Git `atlanticus:main`: implementación.
- `atlanticus-decisions`: decisiones/qualification/rationale.
- Contexto canónico: estado sintetizado vigente.
- Referencias externas: solo comparativas.

Git es read-only salvo autorización explícita.

## Diseño

- debatir antes de implementar;
- exponer supuestos y riesgos;
- contratos antes que consumidores;
- backend antes que frontend;
- composición explícita;
- modularizar solo con responsabilidad/reutilización/variación/frontera real;
- sustituir legacy limpiamente cuando se acuerde un cambio de raíz;
- no expandir scope.

## Código

- simple, legible, mantenible y eficiente;
- funciones para lógica simple sin estado;
- clases solo con estado/ciclo de vida/configuración/estrategia/reutilización real;
- evitar wrappers vacíos, interfaces especulativas y microservicios por moda.

## Operación

Aceptar procesos automáticos, semi-automatizados y manuales controlados.

## Tests

Probar resultados e invariantes, no implementación interna ni CSS visual.

## Seguridad

- Entra ID cuando corresponda.
- Key Vault/credenciales apropiadas.
- nunca secretos en código/logs/errores/telemetría.

## Observabilidad

Separar logs locales y telemetría Azure. Exportar lo útil, no volumen por volumen.

## Entregables

Cada incremento debe acercar una salida verificable. No continuar refinando arquitectura si no existe un bloqueo real para el siguiente entregable.

## Web capability independence

- Users/Profile, Navigation, User Activity, Manager y otras capabilities deben poder componerse selectivamente.
- Ninguna capability incorpora otra como dependencia sólo porque una aplicación use ambas.
- Cross-capability behavior pertenece a composition/binding packages.
- Dashboard/read models pueden agregar información sin crear dependencias circulares.

## Web startup and deployment

- Web es el orquestador de preparación de recursos/proyecciones de aplicación.
- Web no es runtime coordinator de jobs.
- Backend declara requirements neutrales y no depende de packages Web.
- Local puede crear DB/containers cuando el contrato lo permita.
- Cloud database debe preexistir.
- Container schema/TTL mismatch falla explícitamente.
- Backend jobs no provisionan/validan schema en cada ejecución.
- Web debe poder existir sin datos/backend; distinguir availability de readiness.
- Deployment sequence preferida: base infra → Web → resource/projection bootstrap → Backend.

## User Activity

- TTL funcional: 24 h.
- Preservar historia ordenada por página/visita.
- No persistir cada heartbeat como historia.
- Navigation enrichment es integración opcional.

## Manager bootstrap

- No usar `is_local` como full-access bypass.
- Primera instalación usa superficie previa a Manager.
- La superficie puede ser independiente de Users/Profile, pero acciones privilegiadas no son anónimas en producción.

## Controlled reprocessing

- Los jobs incrementales pueden exponer `reprocess current` para repair/testing.
- El modo no elimina authority, ordering, fencing o leases.
- Nunca mover watermark hacia atrás.
- Nunca sobrescribir durable write-once conflict silenciosamente.
- Preferir flags específicas por proceso.
- Default false.
- Historian puede reconstruirse desde durable KPI evaluations.

## Distribution boundary

- Atlanticus owns generation of validated artifacts and distribution-ready packages.
- Atlanticus does not own the corporate DevOps pipeline implementation.
- Generators must not hardcode external pipeline topology.

## Projection model

- Base projections are independent.
- Derived resolutions may declare dependencies.
- Never create circular authority between projections.

## Documentation/productization

- env.detail explains every variable and allowed content.
- stable components receive real READMEs.
- loaders are part of finished product.
- scripts remain component-oriented plus a master integrity aggregator.
- pedagogical University cases are separate from tests.
