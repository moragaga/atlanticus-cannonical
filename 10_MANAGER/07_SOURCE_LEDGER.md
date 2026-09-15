# Manager — Source Ledger

Estado: **AUDIT LEDGER**

## Autoridad

- `moragaga/atlanticus:main` = realidad implementada.
- `moragaga/atlanticus-cannonical:main` = autoridad documental vigente.
- `moragaga/atlanticus-decisions` = HISTORICAL.

Git permanece READ ONLY salvo autorización explícita.

## Fuentes históricas relevantes

Entre las fuentes históricas inspeccionadas:
- `ATLANTICUS_MANAGER_GLOBAL_RULES_2026-09-02.md`;
- `ATLANTICUS_MANAGER_DECISION_ADDENDUM_2026-09-02.docx`;
- `ATLANTICUS_MANAGER_UX_RUNTIME_CONTRACT_2026-09-02.docx`;
- `ATLANTICUS_WEB_UI_COMPOSITION_CSS_BOUNDARY_DECISION_2026-09-03.docx`;
- `Atlanticus_ADA_Web_Structural_Migration_and_Qualification_Plan.docx`;
- `Atlanticus_ADA_Upgrade_Source_Blob_Trabajo_Pendiente.docx`;
- `Atlanticus_ADA_Usuarios_Perfiles_Acceso_Arquitectura_2026-09-10.docx`.

Ninguna fuente histórica no revalidada puede contradecir por sí sola `atlanticus:main` o canonical vigente.

## Implementación actual inspeccionada

- `web/capabilities/manager`;
- `web/capabilities/projection/core`;
- `web/capabilities/source/core`;
- `web/capabilities/users/configuration`;
- `web/compositions/users-manager`;
- `web/compositions/navigation-activity`;
- `scopes/ada/web/application/ada-configuration-manager`.

Checkpoint actual de este cierre:

```text
moragaga/atlanticus@7ffebdbb0b70e41c6f0bd903cc7f27dbd3a05d98
parent: 567e1a12c862b46dfd7f4ec75c3be750c95bbd54
```

`atlanticus:main` fue verificado apuntando a ese commit.

## Checkpoints preservados

Root exact Projection handoff:

```text
5fd2858c4bd19c8f9cc416e0996162cb7a3f8c06
```

Generic Manager exact-source boundary:

```text
9342769a626c39d1f7f860f81e051e2ef1300620
```

Users admin draft baseline semantics:

```text
567e1a12c862b46dfd7f4ec75c3be750c95bbd54
```

Users Manager exact-source composition:

```text
7ffebdbb0b70e41c6f0bd903cc7f27dbd3a05d98
```

## Qualification observada para `7ffebdbb...`

Reportada por el usuario sobre workspace real:

```text
uv lock --check                         GREEN
focused composition tests              7 passed
Ruff src/tests                          GREEN
full Web suite                          587 passed, 7 skipped
git diff --check                        GREEN
Python runtime                          3.14.7
```

No se afirma CI remoto adicional.

## Hallazgo de composición refinado

La afirmación histórica “no se encontró un adapter Users↔Manager” queda refinada.

CURRENT:
- sí existe `web/compositions/users-manager`;
- sí existe `UsersManagerExactSourceWorkflow`;
- su responsabilidad es adaptar `UsersProfilesAdministrationService` al protocolo exact-source de Manager.

Sin embargo, el host ADA productivo todavía registra:

```text
UsersManagerWorkflowAdapter(dependencies.users)
```

Ese adapter legacy:
- vive en el scope ADA Configuration Manager;
- consume `UsersConfigurationCatalog`;
- publica con `expected_source_revision: str | None`.

Por tanto no deben confundirse:
- **composition exact-source disponible**;
- **productive service cutover**.

## Composition boundary

`web/compositions` ya existía antes de este hito.

`navigation-activity`:
- conecta Navigation con el contrato `ActivityRouteResolver`;
- no hace a Navigation owner del tracking;
- Users Activity conserva ownership de sesiones/tiempo/rutas.

`users-manager`:
- conecta Users Configuration con Manager exact-source;
- evita dependencia Manager→Users;
- evita dependencia Users→Manager.

## Pending deep read

Documentos históricos todavía no extraídos textualmente permanecen como fuentes pendientes, no se asumen superseded.

Ningún documento histórico no revalidado puede reemplazar contratos implementados y canónicos vigentes.
