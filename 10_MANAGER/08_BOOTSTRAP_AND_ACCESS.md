# Manager — Bootstrap and Access

Estado: **CURRENT / LOCAL VISUAL INTEGRATION CLOSED / DURABLE E2E OPEN**

## Fronteras vigentes

`BOOTSTRAP ACCESS ≠ MANAGER ACCESS`. Identity/Users rechaza identidades inválidas y usuarios promovidos deshabilitados; una identidad autenticada válida sin `UserRecord` puede llegar a `READY`. Manager aplica `ManagerPrincipal`, `ManagerModule.access_key` / `ManagerEntry.access_key` y `ManagerAuthorizationPolicy.can_view(...)`. El predicado por defecto no concede acceso sin key ni sólo por `is_local` o perfil administrador: requiere la key explícita en `principal.access_keys`.

Capacidades ADA: `users.manage`, `profiles.manage`, `access.manage`, `navigation.manage`, `tools.manage`, `kpis.manage`. `Users` sigue siendo `ManagerEntry`, sin Source/Projection ficticios; `Profiles` es genérico, aunque ADA presente la etiqueta `Perfiles`. `Access` conserva su dominio ADA y grants explícitos para `basic/guest/custom`; las reglas irrestrictas de `root/local` pertenecen al dominio Access y no constituyen un bypass del Manager genérico.

## Identidad local en ADA Generic

El CLI de ADA Generic consulta `ATLANTICUS_LOCAL_IDENTITY_SUBJECT_ID` sólo si está definido; de lo contrario usa `select_local_user()`, cuya elección aleatoria selecciona Jane o John **una vez por arranque de proceso**, no por request ni por recarga del navegador. Fijar un subject explícito es únicamente un recurso determinista de pruebas, no requisito de ejecución local. LocalIdentityProvider no es un sustituto de identidad Entra productiva.

Los colores se resuelven desde las definiciones autorizadas de `atlanticus.web.users.local` por `subject_id` reconocido y contexto `local` comprobado. En Navigation operacional local: Jane `#C85D91` en avatar **e insignia Local**; John `#3778C2` en ambos. El texto es blanco. No duplicar la paleta en la UI ni asignar estos colores a usuarios administrados por coincidencia de nombre o subject. Usuario local desconocido y usuario público conservan sus respectivos fallbacks.

Retirar `principal.display_name` del header de Manager fue un cambio **de presentación**, no de principal, autenticación, autorización ni datos de sesión. El menú operacional ADA puede continuar representando identidad y avatar.

## Estado

- Selección y contrato local: **CURRENT / VERIFIED STATIC**.
- Presentación Jane/John y selección automática: **CLOSED / VERIFIED MANUAL** por el usuario.
- Identidad productiva real: **PLANNED / UNVERIFIED**.
- Manager durable Blob/Cosmos real y restart: **PLANNED / UNVERIFIED**.
- Antiguo hallazgo de un consumer `can_access` frente a `can_view`: **HISTORICAL / requiere revalidación sobre HEAD antes de etiquetar conflicto vigente**; no introducir alias o shim.

Los cierres previos de UI de Access, Profiles y Users no se reabren.
