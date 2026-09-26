# ADA Generic — Current Composition

Estado: **CURRENT / STAGE 1 CLOSED / NAVIGATION LOCAL CLOSED**

Implementación inspeccionada: `moragaga/atlanticus@a6061ffed59c8b04e64b0a7fdc17050ef463c850`.
No se ejecutaron pruebas del repositorio remoto desde este cierre.

## Composición general

ADA Generic integra branding, shell/navigation, header, alarm surfaces, content state,
operational render binding, operational state, runtime experience, global indicators,
time status y consumos configurados. Las capacidades conservan ownership independiente.
Atlanticus core no depende de ADA.

## Bootstrap Tool y Collector — CURRENT

```text
AdaGenericSettings
→ Tool persistence settings
→ optional Storage client / Tool Projection Cosmos client
→ ToolPersistenceComposition
→ resolve_operational_tool_projection()
→ Tool resolution READY | UNCONFIGURED | UNAVAILABLE | INVALID
```

Un resultado no READY mantiene la Web base y expone el estado correspondiente. No recurrir
a Source ni a otro proveedor de manera silenciosa. Una Projection Tool válida puede
consumirse sin Source disponible.

Cuando Tool está READY y KPI Delivery Cosmos está configurado:

```text
Tool Projection → ToolStructure
→ create_operational_kpi_collector()
→ attach_operational_kpi_collector()
→ create_web_application()
```

Tool Projection y KPI Delivery conservan configuraciones de consumo separadas. Collector
realiza polling asíncrono con cache de proceso; el navegador no consulta Cosmos inline.
`OperationalRenderBinding` representa estructura, no snapshots KPI.

```text
1 ToolComponent → 1 dcc.Store KPI
0..N Subcomponents → sin Store adicional
```

La representación específica corresponde al consumidor/desarrollador de la Tool. No hay body
universal obligatorio ni acoplamiento Collector → OperationalRenderBinding.

## Identity, Manager y Navigation — CURRENT

- La composición operacional base monta Navigation y su autorización sin exigir Identity.
- Navigation inicia vacío sin projection configurada; la Home sigue siendo accesible.
- Manager se integra explícitamente cuando existen dependencies/stores; un `ManagerPrincipalBinding`
  local puede necesitar Identity explícita y el bootstrap la agrega conforme al entorno.
- La definición Navigation se lee desde la misma `navigation_projection_store` compartida con
  Manager, empleando `NAVIGATION_SOURCE_KEY`; no se usa un menú fijo como autoridad.
- Principal público sin perfil administrado cuando no existe binding.
- `root` administrado y Local confiable admiten `administrative_override` según el contrato
  implementado; un usuario desconocido o sin privilegios no hereda la excepción.
- La autorización de navegación de documentos HTML no reemplaza el control de acceso Manager.
- No se transfiere ownership de Profiles, Users o ADA Access a Navigation.

## Presentación del menú CURRENT en a6061ffe

```text
ADA operational layout
├── Header + desktop/mobile triggers
├── Navigation controller fuera del Offcanvas
│   ├── dcc.Location
│   └── dcc.Store(last pathname)
├── Navigation Offcanvas
└── Main
```

El callback diferencia inicialización, primera pulsación y cambio real de ruta. Los triggers
no tienen `title='Abrir navegación'`; conservan texto `visually-hidden` accesible.
El comentario pedagógico no es runtime alternativo ni contrato legacy.

## Evidencia y alcance

**VERIFIED AUTOMATED previamente reportado:** después de la integración principal, ADA Generic
`169 passed` y Ruff verde; durante el correctivo del menú, ADA Generic `172 passed` y Ruff
verde. Las pruebas del shell informadas antes de la corrección final: `8 passed`,
`1 skipped` y un error Ruff en el test añadido. No atribuir estos resultados al commit final.

**VERIFIED MANUAL declarado por el usuario:** guardado, publicación, proyección y consumo del
menú desde Home, y menú funcional después del último correctivo en `a6061ffe`.

**UNVERIFIED:** ejecución íntegra de tests/Node y Ruff en `a6061ffe`; recuperación durable
tras reinicio sobre Blob/Cosmos real/emulado; matriz responsive; entrega Azure/Entra.

## Estados

```text
ADA GENERIC STAGE 1                          CLOSED / CURRENT
ADA GENERIC NAVIGATION INTEGRATION            CLOSED / CURRENT
LOCAL NAVIGATION PUBLICATION/PROJECTION       CLOSED / VERIFIED MANUAL
FIRST CLICK/TOOLTIP CODE CORRECTION           CURRENT / USER-REPORTED FUNCTIONAL
REAL PERSISTENCE/DISTRIBUTION QUALIFICATION   PLANNED / UNVERIFIED
```
