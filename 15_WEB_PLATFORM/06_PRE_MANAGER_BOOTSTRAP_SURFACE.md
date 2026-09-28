# Web Platform — Pre-Manager Bootstrap and Isolated Projection Surface

Estado: **BASELINE ANTERIOR CONSERVADA / NUEVA SUPERFICIE AISLADA DECIDIDA COMO DIRECCIÓN / IMPLEMENTACIÓN PLANNED**

## Problema vigente

UAT/PRD pueden iniciar con Sources preparados en Storage, pero sin Profiles, Access ni usuarios promovidos en Cosmos. Ingresar al Manager para publicarlos/proyectarlos exigiría autorizaciones aún inexistentes: dependencia circular. El nuevo flujo resuelve **proyección**, no edición de configuraciones ni una nueva administración de usuarios.

## Dirección del cierre 2026-09-27

Superficie **independiente de Manager** y de las autorizaciones todavía no proyectadas:

```text
Sources preparados en Storage del ambiente
              |
página de proyección aislada (credencial propia)
              |
verificación / estado previo de los módulos
              |
confirmación explícita
              |
proyecciones de las configuraciones existentes
              |
Users: proceso especial de validación/recuperación desde fuente aprobada
              |
resultado por módulo + reintento controlado
              |
Manager normal, sólo cuando su autorización ya existe
```

No es una pantalla que permita editar `Users`, `Profiles`, `Access` u otros módulos; no da acceso a `/manager` ni intenta sustituir los workflows Source/Projection de cada dominio. No agregar un segundo sistema de publicación. No suponer un orden global rígido: respetar dependencias reales de cada proyección y verificar disponibilidad de sus Sources (`07_PROJECTION_ORCHESTRATION.md`).

## Credenciales y material protegido — requerimientos decididos, mecanismo OPEN

- El equipo del proyecto prepara el material mediante tooling; el usuario de servicio y la contraseña se indican al ejecutar la operación.
- La credencial **no caduca automáticamente** ni queda consumida con un primer uso; debe poder reintentarse sin regenerar el paquete ante fallo.
- Si la contraseña se pierde, el equipo regenera material y redistribuye/reinicia conforme al mecanismo que llegue a implementarse. No se promete recuperar la contraseña original.
- El hash se calcula/verifica según el mecanismo criptográfico que se acuerde. Un hash irreversible **no descifra** datos: si se exige contenido protegido, necesita cifrado con una clave derivable u otro mecanismo formal.
- No guardar contraseñas en texto claro, crear sesiones administrativas indefinidas ni incluir secretos productivos en el Starter.

**OPEN:** forma exacta del archivo, derivación/verificación, custodia del verificador, aislamiento de credenciales respecto a infraestructura, bootstrap anterior a Users, política de autorización de cada acción y aceptación de reintentos/fallos parciales. No inventar implementación antes de decidir estos contratos.

## Compatibilidad con baseline anterior

La dirección canónica anterior establecía un login real (Entra en producción) y una consola general de bootstrap previa al Manager. Este cierre **refina el alcance inmediato**: sólo una página aislada de proyección, protegida por la credencial del proyecto, sin dashboards generales de infraestructura ni administración remota completa. El requisito de identidad productiva normal del Manager **permanece** y no debe interpretarse como habilitado por la página.

**CONFLICT / OPEN contractual:** la exigencia anterior de Entra para toda superficie pre-Manager y la nueva autorización independiente mediante servicio requieren una delimitación explícita antes de implementar el acceso productivo. No resolver por supuesto ni exponer acciones sin controles.

## Estado

`ISOLATED-CONFIGURATION-PROJECTION-PAGE`: **PLANNED / NO CODE VERIFIED**.
`USERS-PROJECTION-RECOVERY`: **NEXT SEPARATE INCREMENT**.
`PRODUCTION-IDENTITY-PROVIDER`: **UNVERIFIED / SEPARATE**.
