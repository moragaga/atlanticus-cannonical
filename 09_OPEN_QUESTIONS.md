# Atlanticus — Open Questions

Estado: **CANONICAL OPEN ITEMS**

Estos puntos no bloquean el cierre del bootstrap. Se resuelven durante ejecución cuando corresponda.

## User Activity

1. partition key final;
2. deterministic id format;
3. exact session persistence mechanism across reload;
4. verify TTL 86400 physically.

## Resources

5. complete resource/container inventory after tracing Operaciones Integradas;
6. Storage provisioning parity;
7. cloud permissions for application resource creation.

## Source / Blob

Cerrados por `SOURCE-1A.1` / `SOURCE-1A.2`:
- release identity;
- release granularity;
- functional manifest;
- SourceStore API;
- concurrency/current;
- Blob provider parity/recovery.

13. retention/cleanup policy permanece abierta fuera de `SourceStore`.

## Backend generation

14. freeze ProcessDefinition/generator current implementation after audit;
15. distribution metadata expected by external DevOps;
16. finish Python 3.14.7/Trixie migration.

## Frontend generation

17. reconcile/generate current ADA Generic artifact tooling;
18. final Web distribution contract.

## Manager

19. remove local full-access bypass;
20. implement Login/Bootstrap Console;
21. freeze Component External Links schema;
22. warmup/cache contract for links.

## KPI

23. implement/test common `REPROCESS_CURRENT`;
24. verify full Historian rebuild semantics on partial/missing materialization.

## Alarm / Command Center

25. finish B.2 runtime/materialization gap;
26. freeze Management payload/actions for ADA Web;
27. run History/Data Sufficiency qualification;
28. only then decide additional events/evidence;
29. analytics remains deferred until GREEN.

## Alarm preservation

30. detailed extraction of some early A-D qualification genealogy remains preservation backlog.
   Original sources remain retained; this does not block product work but must not be discarded.

## Scripts / Docs

31. component-level gates still missing in some areas;
32. master repo integrity gate;
33. env.detail enrichment;
34. final READMEs.

## Loaders

35. implement approved ADA loader;
36. Manager loader;
37. Command Center loader when its Web design closes.
