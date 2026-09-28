# Alarm Engine — Runtime and Lifecycle

Estado: **CURRENT — Core, WAL/adopción B2a/B2b, ejecución/composición B2c, publicación Engine B2c.7a/d; qualification física separada**. Corte 2026-09-28. Reglas físicas caracterizadas históricamente; B2c.7 no las rediseñó. **Código del hito verificado por lectura remota en** `atlanticus@c67fcb5b105cc561c16719a8bca4ea5aa74c3fae`; `main@bc1d73742bcb04eb495bbbb1725a8ad23d4eff38` está un commit posterior con cambios sólo de ADA Generic Master Projection, fuera de este alcance. Los gates locales son evidencia del usuario, no CI de este checkout.

## Cycle boundary

`reduce_group_cycle` recibe estado durable de grupo, `cycle_at`, `PlannedAlarm`, evaluaciones de ciclo, closures de configuración, acciones Management, deactivation y factories/resolvers explícitos. No depende de estado global. La sesión efectiva permanece fijada durante la ejecución del job y el reducer no controla configuración ni visualización Web.

## Orden lógico de Core CURRENT

```text
1. validar cycle/configuration inputs
2. indexar PlannedAlarm y evaluaciones
3. preparar Management/deactivation inputs
4. resolver lifecycle físico/técnico y occurrences
5. construir siguiente GroupLifecycleState
6. finalizar Management/deactivation (timers, reappearance Special Condition,
   expiración, scope cleanup y CascadeSuppression)
7. resolver routing
8. resolver priority
9. emitir GroupLifecycleDecision
```

La reappearance por Special Condition se considera antes de routing y priority finales. Management/deactivation no redefinen la condición física: una occurrence puede estar físicamente ACTIVE, gestionada y/o deactivated. `CascadeSuppression` sólo afecta disposición operacional.

## Cascade suppression y deactivation barrier CURRENT

Dos fuentes causales: `ManagementEffect` activo o `DeactivationEffect` activo, con `management_effect_id XOR deactivation_effect_id` (exactamente uno). En concurrencia, deactivation domina atribución. Menor `priority_order` equivale a prioridad mayor; sólo targets activos del mismo `priority_group` con `priority_order` superior al de la source son elegibles. `kind` y visibility no deciden elegibilidad. Esto permanece en tensión documental con ciertas formulaciones B.1 Special Cascade; **CONFLICT** sin reconciliación formal en decisions.

Mientras una deactivation esté vigente: la source queda `DEACTIVATED` y los targets elegibles `CASCADE_SUPPRESSED`. Pending approval no crea efecto; la expiración elimina la barrera y recalcula priority. El efecto puede persistir ante cambio de occurrence mientras siga vigente conforme al contrato preexistente.

## Reappearance y routing

Management puede terminar por timer o Special Condition. Si continúa deactivation, no se atraviesa la barrera: el ManagementEffect puede limpiarse, la deactivation permanece, no se emite `ReappearanceChange` en el camino caracterizado y los targets elegibles continúan suprimidos. Reappearance temporal+SC en el mismo ciclo no debe duplicar cambios. Una Rule cerrada no revive automáticamente. Routing sigue progresando según su contrato aun con gestión, eclipse, suppression o deactivation; esta última no pausa C2 por defecto.

## Priority CURRENT

`PREDOMINANT`, `ECLIPSED`, `CASCADE_SUPPRESSED`, `DEACTIVATED`. No reintroducir `SHADOW` ni `delivery_enabled` a Runtime Core. Live futuro aplicará el filtro ya acordado: VISIBLE + {PREDOMINANT,DEACTIVATED}, sin permitir Web recalcular prioridad.

## Reconfiguration/adoption — estado refinado

B2a/B2b mantienen WAL global adoption V1 (cero grupos) y V2 (1..N grupos), EFFECTIVE derivado y exact read; ambos formatos del WAL son CURRENT. La descripción anterior según la cual `adoption_execution.py` nunca llama `commit_adoption` ha quedado **SUPERSEDED** por la implementación inspeccionada: ejecutor y `configured_iteration.py` integran bootstrap/adopción y sesión fijada. `application.py`/`bootstrap.py` aportan composición operativa con puertos existentes; la existencia de wiring no demuestra distribución/Docker/productivo físico.

B2c.7 añade un **read-side de publicación**, no altera Core: tras confirmar commits requeridos, `operational_runner.py` publica CURRENT v1 y FACTS runtime v2 encadenados. El receptor Delivery está en otro job; consume archivos de salida, sin compartir estado mutable ni importar internals ejecutables del Engine.

**OPEN / separado:** reconciliación de `reappearance_after_seconds` y `reappearance_special_conditions` respecto de `ManagementEffect` todavía vigente, así como cambios de prioridad/grupo/kind/evaluator divergentes entre B.1 y `adoption.py`. No resolverlos como efecto lateral de distribución.
