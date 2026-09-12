# Atlanticus — Operating Model

Estado: **CURRENT**

Atlanticus no persigue automatización total como objetivo independiente.

## Modos válidos

### AUTOMATED
El sistema completa el proceso sin intervención humana normal.

### SEMIAUTOMATED
El sistema prepara/valida/materializa, pero existe una decisión, creación o aprobación humana legítima.

### MANUAL CONTROLLED
La creación/decisión es humana, con contratos, validaciones, persistencia y auditoría del sistema.

## Regla

Un paso manual no es deuda técnica por el solo hecho de ser manual.

Para cada flujo importante documentar:

- qué crea o decide una persona;
- qué valida el sistema;
- qué se persiste;
- qué se publica;
- qué puede reintentarse;
- qué falla cerrado;
- qué queda auditado.

Manager/configuration/authoring debe evaluarse bajo este modelo y no asumirse como candidato a automatización total.

## Deployment operating model

El despliegue también tiene pasos automáticos, semiautomáticos y administrados.

### Cloud base infrastructure

Normalmente controlado por soporte/IaC:

- accounts;
- database;
- storage account;
- networking;
- identity;
- secrets.

### Application preparation

Responsabilidad Web:

- validar configuración;
- crear/validar application containers;
- reunir requirements externos;
- proyectar configuración guardada;
- exponer readiness.

### Backend activation

Ocurre después del bootstrap Web.

Los jobs consumen infraestructura preparada y no vuelven a provisionarla en cada ejecución.

### Primera instalación

La superficie pre-Manager resuelve el chicken-and-egg de Users/Profile sin entregar permisos anónimos.
