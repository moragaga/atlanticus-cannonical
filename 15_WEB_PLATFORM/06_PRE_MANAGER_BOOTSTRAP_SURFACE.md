# Web Platform — Pre-Manager Bootstrap and Isolated Master Projection Surface

Estado: **CURRENT — MATERIAL 001A, PLANNER 001B, HTTP 001C Y APPLY 001D.1/001D.2 IMPLEMENTADOS; 001D.3 NAVIGATION DOCKER LOCAL CLOSED; MASTER USERS REPLACE BLOCKED**.  
Corte específico: `moragaga/atlanticus@9c6daffd04b9c249f75a55b6cdb9b44e6d92a795`; canonical base `0f2fff3ec0e71903b5703e03dd6050765d9722ff`. No atribuir pruebas antiguas ni de otros ámbitos a este HEAD.

## Por qué existe y qué no hace

Con Profiles/ADA Access/usuarios promovidos aún ausentes, el Manager puede carecer de acceso administrativo ordinario. Master es una superficie **independiente de servicio en la Web existente**, no un módulo del Manager ni un nuevo proceso. Permite inspeccionar y ahora **aplicar de forma individual** las seis proyecciones ordinarias existentes desde Sources **ya publicados**. No crea/publica Sources, no otorga permisos Manager y no ejecuta Users REPLACE.

## 001A — material protegido CURRENT

```text
tooling/distribution/web/starter/ada/tooling/master_projection.py
tooling/distribution/web/starter/ada/src/application/master_projection/{material,reader}.py
```

El generador del Starter ADA solicita contraseña por consola interactiva; produce **fuera de la distribución** un ZIP AES-256 con verificador `scrypt`, `service_user`, `material_id`, `application_namespace`, `environment` y las acciones declaradas `projection.preview`, `projection.apply`, `users.replace`. El archivo real es sensible: nunca incorporarlo ni registrar la contraseña en Git, imagen, Starter, wheelhouse o `.env.detail`; no se consume al iniciar sesión. El lector señala `ABSENT`, `PRESENT`, `INVALID`, con fingerprint SHA-256 para invalidar sesiones si cambia el material.

La única configuración adicional de Master implementada es `ADA_MASTER_PROJECTION_MATERIAL_PATH`, ruta **opcional, absoluta y externa** al proyecto. Ausencia: página informativa sin acciones; invalidez: respuesta controlada; presencia: login propio. No inferir pipeline productivo de material desde ese contrato.

## 001B y 001D — separación lectura/escritura

El planner usa seis pares existentes `Navigation`, `Profiles`, `Tools`, `ADA Access`, `KPI Registry`, `KPI Definitions`; prerrequisitos Access→Profiles, KPI Registry→Tools y KPI Definitions→KPI Registry. Conserva `ProjectionTarget.dependencies` completos. Inspeccionar un estado READY no autoriza aplicar: `MasterProjectionExecutor` ejecuta un **único target exacto** después de validación actualizada y verifica persistencia.

`GET/POST /master-projection` presenta el plan e incorpora prepare/confirm explícitos solo a sesiones con `projection.apply`; selección pendiente exacta más nonce en sesión, CSRF, nueva consulta del plan al confirmar y nueva comprobación del target en backend. Repetir un target ya alineado devuelve `ALREADY_CURRENT`. Los resultados/fallos no prometen rollback distribuido.

La sesión Master expira a los 900 segundos, la sustitución del material revoca la sesión por fingerprint y logout es `POST /master-projection/logout` con CSRF. Estas **dos rutas exactas** se interceptan antes de Identity/Navigation. Una ruta bajo ese prefijo que no coincida exactamente continúa su autorización normal; no ampliar la excepción. El usuario de servicio no recibe acceso `/manager`.

Users se muestra **aparte y no ejecutable** desde Master; el servicio especial del Manager conserva captura de snapshots aprobados, RESTORE/REPLACE y sus propias restricciones de seguridad.

## Qualification por versión

- **VERIFIED USER-REPORTED histórico 001C:** pruebas HTTP/Reader/Runtime/settings acotadas en distintos checkpoints y smoke sin/con material. El build 001C previo al commit tenía modificaciones locales: no atribuirlo bit a bit al SHA anunciado en su manifest.
- **VERIFIED USER-REPORTED 001D.2:** 50 tests seleccionados + Ruff + diff check PASS, publicados en `ca3ee508`.
- **VERIFIED USER-REPORTED 001D.3:** distribución limpia vinculada a `ca3ee508`, 67 wheels, precheck, sync y material real; sesión Master alcanzada. En Docker local, al cambiar/publicar Navigation en Manager y aplicar mediante prepare/confirm en Master, el Manager recargado indicó **proyectado y sincronizado**. No hubo necesidad de borrar Cosmos.
- **UNVERIFIED:** prueba real de cada uno de los otros cinco dominios, producción con Entra/Azure, multiworker, rotación/revocación operativa y fault injection.

### Evidencia histórica 001C que no se debe borrar

El primer artifact smoke 001C falló HTTP 500 debido al orden Navigation/Identity para la ruta Master exceptuada; el middleware fue reordenado y se incorporaron tests de integración HTTP. El smoke posterior 001C registró 67 wheels y HTTP 200 con y sin material real, pero se construyó desde un working tree previo al commit: su manifest no acredita por sí solo la identidad exacta del código empaquetado. Suites seleccionadas previas de 38/38, 39/39, 3/3, 6/6 y 2/2 pertenecen a **momentos distintos** y no se agregan ni se reatribuyen al HEAD 9c6.

## Límites contractuales OPEN

La baseline histórica Entra pre-Manager y la excepción de credencial Master representan una **tensión reportada en la canonical histórica**, cuya resolución requiere revalidar decisiones DOCX antes de producción; **no** se ha determinado aquí que los documentos de decisiones vigentes estén formalmente en conflicto. Warmup/upload productivo, custodia y rotación no están implementados/verificados como flujo completo.

`users.replace` permanece **BLOCKED** desde Master: el provider actual deriva `identity_realm` del único issuer entre usuarios promovidos, por lo que no resuelve por sí mismo un destino vacío. No usar Registry de candidatos como snapshot aprobado, inventar defaults, adapters legacy o bypass de Identity.

**SUPERSEDED:** afirmaciones anteriores de página Master exclusivamente read-only y de Apply futuro. El plan sigue siendo read-only, **la página ya dispone de ejecución individual controlada**. El siguiente foco documental es consolidación canonical, no nuevo código.
