# ADA Command Center — Identity, Navigation, Profiles and Activity

Estado: **CURRENT DIRECTION**

## Identity

Producción usa Microsoft Entra ID.

Reutilizar capability transversal Atlanticus.

No crear autenticación paralela.

## Capability independence

Command Center puede integrar:

- Users/Profile;
- Navigation;
- User Activity;

de manera independiente.

No establecer:

```text
Navigation requires Users
Activity requires Navigation
Users requires Activity
```

Bindings opcionales resuelven integración cuando exista.

## Profiles

Profiles/permissions pueden controlar:

- consulta;
- Configuration;
- publicación;
- management/deactivation;
- capacidades futuras.

Los nombres exactos no están congelados.

## Navigation

Reutilizar capability transversal.

Puede funcionar sin Users/Profile.

Si existen restricciones por perfiles:

```text
optional profile-navigation composition
```

## Users

Entra entrega identidad.

Users/Profile resuelve identidad hacia perfil/acceso cuando la aplicación lo instala.

No crear modelo duplicado de usuarios.

## User Activity

User Activity pasa a ser una capability **opcional e integrable**, no una dependencia obligatoria ni una prohibición del primer diseño.

Si Command Center la activa:

- usa mismo contrato transversal;
- historia ordenada por página;
- TTL 24 h;
- no depende obligatoriamente de Navigation;
- Dashboard puede unificar actividad de uso con sus propias vistas sin acoplar domains.

No es requisito para que Alarm Engine ni Analytics funcionen.
