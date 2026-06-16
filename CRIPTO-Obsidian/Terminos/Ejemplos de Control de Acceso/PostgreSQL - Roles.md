PostgreSQL utiliza un modelo de control de acceso basado en **roles**, una implementación de [[RBAC]].

## Características

- Un rol puede estar asociado a:
  - Un **usuario** individual.
  - Un **grupo de usuarios**.
  - **Otro rol** (herencia de roles).
- Los roles poseen **atributos** que definen sus capacidades.
  - El atributo `LOGIN` permite iniciar una sesión con la base de datos.
- Todos los objetos creados por un rol tienen a ese rol como **owner** → implementación de [[Discretionary Access Control]].
  - Solo los superusers y el owner pueden acceder al objeto por defecto.

## Creación de roles

```sql
-- Crear un rol con permisos de login
CREATE ROLE analista LOGIN;

-- Asignar permisos sobre una tabla
GRANT SELECT ON TABLE ventas TO analista;
```

## Relación con otros conceptos

- [[RBAC]]: PostgreSQL implementa control de acceso basado en roles.
- [[Discretionary Access Control]]: el owner del objeto controla quién accede.
- [[PostgreSQL - Policies]]: extiende el control a nivel de filas dentro de las tablas.

## Referencia

https://www.postgresql.org/docs/current/user-manag.html
