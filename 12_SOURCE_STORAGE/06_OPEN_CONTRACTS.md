# Source Storage — Open Contracts

Estado: **OPEN**

Antes de implementar hay que cerrar:

1. Identidad de release.
2. Granularidad:
   - historia global;
   - por familia;
   - otra división explícita.
3. Functional manifest.
4. Physical layout Blob/Local.
5. `SourceStore` operations/results/errors.
6. Conditional promotion/concurrency.
7. Compare/history API.
8. `source_release_id` en Projection.
9. Retention.
10. Storage account assumptions:
    - HNS;
    - cuenta reutilizada vs conexión nombrada `configuration_source`;
    - lifecycle policy.

No inferir estos detalles desde Azure ni desde SharePoint legacy.
