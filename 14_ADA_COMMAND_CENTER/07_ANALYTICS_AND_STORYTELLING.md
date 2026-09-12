# ADA Command Center — Analytics and Storytelling

Estado: **DEFERRED UNTIL DATA SUFFICIENCY VALIDATION**

## Objetivo de producto

Command Center debe llegar a explicar:

- qué ocurrió;
- cuánto duró;
- qué evidencia existía;
- management/deactivation;
- reappearance;
- escalation;
- cierre;
- patrones relevantes.

Pero esta capa **no se implementa todavía**.

## Razón

Antes necesitamos demostrar mediante pruebas que el Alarm Engine y sus proyecciones conservan toda la información necesaria.

No asumir que Journey + Evidence actuales son suficientes sólo porque parecen ricos.

## Fase previa obligatoria

Construir casos de qualification sobre historias reales/controladas y responder:

1. ¿podemos reconstruir una occurrence completa?
2. ¿podemos conocer todas sus transiciones relevantes?
3. ¿podemos asociar management/deactivation correctamente?
4. ¿podemos conocer priority/suppression durante la historia?
5. ¿podemos asociar Tool/Component/Subcomponent vigente?
6. ¿podemos recuperar evidencia suficiente para explicar activation/closure?
7. ¿podemos comparar configuration revision?
8. ¿falta algún evento o snapshot?

## Resultado esperado

### Si la información es suficiente

```text
DATA SUFFICIENCY = GREEN
→ congelar History/Analytics read model
→ diseñar dashboard/insights
```

### Si falta información

```text
DATA SUFFICIENCY = GAP
→ agregar sólo el evento/evidence faltante al backend
→ repetir prueba
```

## Regla

No construir analytics sobre inferencias irreproducibles.

Primero:

```text
Engine facts
→ qualification
→ data completeness
```

Después:

```text
read model
→ analytics
→ storytelling
```
