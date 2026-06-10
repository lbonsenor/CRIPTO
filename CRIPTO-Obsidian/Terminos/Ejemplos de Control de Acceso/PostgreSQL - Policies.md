Las **policies** de PostgreSQL permiten la creación de políticas de acceso y modificación de **registros individuales** dentro de las tablas (_Row Level Security_ - RLS).

## Características

- Permiten definir el **need-to-know a nivel de filas**: un sujeto solo puede ver/modificar los registros que le corresponden.
- Implementan un [[Mandatory Access Control]] adicional al modelo de [[PostgreSQL - Roles]].

## Habilitación y ejemplo

```sql
-- Activar Row Level Security en la tabla
ALTER TABLE clients ENABLE ROW LEVEL SECURITY;

-- Crear una policy: el vendedor solo ve sus propios clientes
CREATE POLICY salesperson_clients ON clients
    USING (salesperson_id::text = current_user);
```

En este ejemplo, cada usuario solo puede acceder a los registros donde `salesperson_id` coincide con su nombre de usuario.

## Relación con modelos de seguridad

- Se corresponde con el concepto de **compartimentos** del [[Modelo Bell-La Padula]]: un sujeto solo accede a la información que le corresponde, sin importar su nivel de acceso general.
- Implementa _need-to-know_ a nivel de datos.

## Relación con otros conceptos

- [[PostgreSQL - Roles]]: los roles definen quién puede acceder a qué tablas; las policies definen qué filas dentro de esas tablas.
- [[Mandatory Access Control]]: las policies se aplican obligatoriamente, independientemente de los permisos del rol.

## Referencia

https://pganalyze.com/blog/postgres-row-level-security-django-python
