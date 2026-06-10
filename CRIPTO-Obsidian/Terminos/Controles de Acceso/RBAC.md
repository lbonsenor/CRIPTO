
**Role-Based Access Control (RBAC)** es una variante del [[Control por Comprensión]] donde los permisos se definen únicamente en función de **roles**.

## Características

- Los permisos se asignan a **roles**, no directamente a sujetos.
- Los roles se asignan a **sujetos** (usuarios) o a **grupos**.
- Los roles suelen tener una representación **funcional de la organización**.

## Ejemplos de roles

- Gestor de pacientes
- Analista de compras
- DBA (Database Administrator)

## Ventajas

- Simplifica la gestión: cambiar los permisos de un rol afecta a todos los sujetos con ese rol.
- Facilita la implementación del principio de _least privilege_ (mínimo privilegio).

## Relación con otros modelos

- Es la base del control de acceso en muchos sistemas reales como [[PostgreSQL - Roles]], [[AWS IAM]], y sistemas operativos.
- Puede combinarse con [[ABAC]] para mayor granularidad.
- Contrasta con [[RuBAC]], donde las reglas son más granulares y explícitas.
