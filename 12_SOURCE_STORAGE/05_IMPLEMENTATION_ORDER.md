# Source Storage — Implementation Order

Estado: **CURRENT PLAN**

## Orden

1. Auditar los contratos Source actuales por dominio.
2. Localizar/auditar el functional publication manifest real.
3. Congelar `ReleaseModel`.
4. Congelar `SourceStore`.
5. Congelar concurrencia/promoción de current.
6. Implementar provider Local.
7. Probar:
   - version creation;
   - no-op publish;
   - immutable history;
   - restore-as-new-release;
   - conflict;
   - integrity;
   - recovery.
8. Implementar Blob como segundo provider.
9. Añadir `source_release_id` a Projection.
10. Integrar Manager history/compare/conflict.
11. Migrar dominios progresivamente.
12. Retirar SharePoint/Power Automate del flujo sólo con paridad y recovery validados.

## Regla

No empezar por UI de historial.

La UI debe consumir contratos backend ya estabilizados.
