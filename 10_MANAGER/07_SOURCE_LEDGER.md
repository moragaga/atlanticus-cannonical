# Manager — Source Ledger

Estado: **AUDIT LEDGER**

## Git decisions históricas

- `ATLANTICUS_MANAGER_GLOBAL_RULES_2026-09-02.md`
- `manager_decisions/ATLANTICUS_MANAGER_GLOBAL_RULES_2026-09-02.md`
- `manager_decisions/ATLANTICUS_MANAGER_GLOBAL_RULES_2026-09-02_2.md`
  - duplicado byte-a-byte del anterior.
- `manager_decisions/ATLANTICUS_MANAGER_DECISION_ADDENDUM_2026-09-02.docx`
- `manager_decisions/ATLANTICUS_MANAGER_UX_RUNTIME_CONTRACT_2026-09-02.docx`
- `manager_decisions/ATLANTICUS_WEB_UI_COMPOSITION_CSS_BOUNDARY_DECISION_2026-09-03.docx`
- `manager_decisions/Atlanticus_ADA_Web_Structural_Migration_and_Qualification_Plan.docx`
- `manager_decisions/Atlanticus_Tools_Close_KPI_Configuration_Handoff.docx`
- `manager_dispatched/Atlanticus_ADA_Upgrade_Source_Blob_Trabajo_Pendiente.docx`
- `manager_dispatched/Atlanticus_ADA_Upgrade_Source_Blob_Trabajo_Pendiente_2.docx`
  - duplicado byte-a-byte.
- `manager_dispatched/Atlanticus_ADA_Usuarios_Perfiles_Acceso_Arquitectura_2026-09-10.docx`

`atlanticus-decisions` es fuente histórica; `atlanticus-cannonical:main` es la autoridad documental vigente.

## Library evidence read

- `ATLANTICUS_MANAGER_DECISION_ADDENDUM_2026-09-02.docx`
- `Atlanticus_ADA_Upgrade_Source_Blob_Trabajo_Pendiente.docx`
- `Atlanticus_ADA_Autoridad_Tool_KPI_Render_Alarmas_2026-09-01.docx`
- `ATLANTICUS_MANAGER_GLOBAL_RULES_2026-09-02.md`

La regla global histórica de Manager permanece compatible con el cierre actual: Manager sigue siendo capability genérica, la configuración de dominio permanece separada del workflow y no se introdujo dependencia ADA en el core.

## Current implementation inspected

- `web/capabilities/manager`
- `web/capabilities/projection/core`
- `web/capabilities/source/core`
- `web/capabilities/users/configuration`
- `web/capabilities/users/projection-cosmos`
- `web/capabilities/navigation/configuration`
- `scopes/ada/web/application/ada-configuration-manager`
- `connectivity/storage`

Checkpoint de implementación del cierre:

```text
moragaga/atlanticus@5fd2858c4bd19c8f9cc416e0996162cb7a3f8c06
parent: 139ee93a118e51f66c3d585f00235f212a2475c1
```

`atlanticus:main` fue verificado apuntando al mismo commit.

## Qualification observada para MANAGER-ROOT-CANONICAL-CUTOVER

- `uv lock --check`: GREEN;
- `uv run ruff check capabilities/manager`: GREEN;
- `uv run pytest capabilities/manager/tests -q`: 76 passed;
- `uv run pytest -q`: 514 passed, 7 skipped;
- `git diff --check`: GREEN antes del commit;
- el commit contiene exactamente 9 archivos del alcance Manager: 3 productivos, 3 espejos comentados y 3 tests.

No hay status checks remotos registrados para el commit; la qualification de este cierre es la ejecutada en el workspace local y la inspección read-only del commit integrado.

## Hallazgo documental refinado

No se encontró en `atlanticus:main` una implementación productiva con los nombres:
- `UsersManagerWorkflowAdapter`;
- `NavigationManagerWorkflowAdapter`.

Las referencias canónicas previas a esos nombres quedan refinadas: no deben tratarse como realidad implementada. Las migraciones administrativas de dominio deben auditar la composición real antes de fijar nombres/adapters.

## Pending deep read

Los documentos históricos de este ledger que todavía no hayan sido extraídos textualmente deben mantenerse como fuentes pendientes, no asumirse superseded.

Ningún documento histórico no revalidado puede contradecir por sí solo la realidad implementada de `atlanticus:main` ni el canonical vigente.
