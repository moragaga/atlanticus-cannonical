# Web Platform — Pre-Manager Bootstrap and Isolated Projection Surface

Estado: **PLANNED / NEXT: MASTER PROJECTION / NO IMPLEMENTATION VERIFIED**  
Corte: `moragaga/atlanticus@208c8d6244795ba92cbe6f8e6b11e9743191367d`; `atlanticus-decisions@50c2bb3f7bf21b05444a102d4502250a5c8a7d2e`; canonical anterior `e49901fb3ceef5431edbde1d0dcbb29fc3502855`.

## Problema

Al desplegar un ambiente inicialmente vacío, el Manager normal puede necesitar Profiles, Access y usuarios promovidos para autorizar al operador. La inicialización de estas proyecciones no puede depender de que ya existan. El workflow de Users Recovery/Projection **sí** se implementó en este hito, pero su página pertenece al Manager y el provider durable actual deriva su `identity_realm` de promovidos existentes; eso **no** resuelve por sí solo un bootstrap vacío.

## Siguiente frontera — dirección acordada

**Master Projection** será una **página independiente de Manager**, accesible mediante su propia URL y protegida por credenciales de acceso independientes de Users/Profiles/Access ordinarios. Reutilizará los proyectores y los Sources ya implementados. No agrega rutas de administración configurables por herramienta, ni ofrece edición/publicación de Sources, ni concede acceso implícito al Manager.

El equipo debe poder usar el **tooling de distribución ADA existente** para preparar una operación/generador que, con credenciales de servicio indicadas por el operador, **produzca el archivo protegido que después se subirá/integrará mediante el warmup**. Revisar ese tooling antes de decidir dónde extenderlo. Se verificó estáticamente la presencia de `tooling/distribution/web/ada/build_distribution.py`, `tooling/distribution/web/generate_starter.py`, `tooling/distribution/web/starter/ada/tooling/project.py` y los directorios `tooling/distribution/web/starter/{ada,base}`. **No se verificó un generador actual de material Master Projection/warmup ni su formato**.

## Dos estados obligatorios de la página

**1. Archivo/material de acceso AUSENTE**

- La URL de la página debe poder responder con un estado controlado e informativo: **no existe acceso configurado actualmente**.
- No mostrar un formulario aparentemente utilizable ni permitir autenticación o acción de proyección sin material válido.
- La ausencia no debe provocar una excepción innecesaria que impida diagnosticar el estado de preparación de esta página. Un recurso corrupto o inaccesible debe manejarse de forma explícita y diferenciada, sin revelar secretos.
- No habilitar por defecto un bypass, contraseña de desarrollo, acceso anónimo ni permisos del Manager.

**2. Archivo/material de acceso PRESENTE e íntegro**

- Habilitar autenticación con usuario de servicio y contraseña asociados al material distribuido, después de verificarlo según el contrato de seguridad por decidir.
- Tras autenticación autorizada, inspeccionar qué Sources/proyecciones aplican al ambiente y sus dependencias reales; ofrecer plan/estado/confirmaciones y resultados por componente.
- Ejecutar proyecciones mediante servicios existentes; Users emplea su procedimiento especial basado en **snapshot aprobado**, sin reinterpretar candidatos de Blob como promovidos.
- Mantener resultados auditables y reintentos controlados. No prometer atomicidad entre módulos ni reproyectar componentes que ya están alineados cuando el contrato permite reconocer esa condición.

Estas reglas son **requisitos/decisiones de producto del cierre**, todavía no validación de código de Master Projection.

## Credenciales/material protegido — intención conservada

- El equipo prepara el material con tooling; el usuario/contraseña del servicio se establecen durante ese proceso.
- La credencial no expira automáticamente ni se consume por la primera ejecución; el mismo material permite reintentar tras fallos hasta que sea revocado/sustituido explícitamente.
- Si se pierde la contraseña, se genera y distribuye nuevo material. No prometer descifrar o recuperar contraseñas a partir de un hash.
- Nunca empaquetar secretos productivos en el Starter ni almacenar contraseñas en claro. Si hay contenido confidencial transportado en el archivo, un hash por sí solo es insuficiente: se debe definir cifrado/verificación apropiados.
- La presencia de archivo no es una autorización suficiente: también deben verificarse credenciales, integridad, operaciones permitidas y estado de sesión.

## OPEN que se resuelven ANTES de implementar

1. **Contrato exacto del archivo** y su ubicación de integración con warmup/Starter. No inventar nombre/extensión/esquema hasta revisar los generadores y loaders disponibles.
2. **Verificación de credenciales, cifrado, custodia y rotación** del verificador/material; autorización servidor por operación, sesión y protección ante exposición.
3. **Owner y límites de la página fuera del Manager** dentro de la composición existente; no duplicar shell, runtime ni permisos normales.
4. **Plan de proyección real**: inventario de Sources presentes, dependencias `ProjectionTarget` exactas, orden derivado, estado por dominio, fallos parciales y reintentos.
5. **Users en un destino vacío**: resolver identidad/compatibilidad y snapshot aprobado sin depender del provider de Users Projection del Manager que exige promovidos existentes.
6. **Conflicto contractual anterior**: la baseline previa pedía Entra para toda superficie pre-Manager; este requisito de credencial de servicio aislada debe delimitarse explícitamente antes de abrir una superficie productiva. La identidad Entra productiva ordinaria del Manager sigue siendo separada.

## No confundir

```text
Manager / Proyección de usuarios     CURRENT / para operador ya autorizado
Master Projection por URL           PLANNED / acceso excepcional independiente
Material producido por tooling ADA  PLANNED / contrato y wiring por acordar
```

No implementar Master durante este cierre documental. Siguiente hito: **debate/contrato de Master Projection y su integración mínima con tooling existente**, luego autorización para código incremental.
