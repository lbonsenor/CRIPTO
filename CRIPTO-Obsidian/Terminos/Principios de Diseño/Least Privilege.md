> Principio 1 de [[Principios de Diseño de Seguridad]]

**Menor privilegio** establece que los sujetos sólo deben tener el acceso **estrictamente necesario** para realizar su trabajo.

## Concepto

Cada sujeto (usuario, proceso, servicio) debe operar con el **mínimo conjunto de permisos** que le permita cumplir su función. Todo acceso adicional aumenta innecesariamente la superficie de ataque.

## Ejemplo

En un sistema con información financiera confidencial:

- La **recepcionista** que atiende el teléfono y agenda reuniones probablemente **no necesita** acceder a información confidencial de clientes.
- El **administrador de cuentas** sí necesita acceder, pero **solo a las cuentas que administra**, no a las de otros colegas.

## Relación con otros conceptos

- [[RBAC]]: asignar roles con el mínimo conjunto de permisos necesarios.
- [[Separation of Privilege]]: complementa el menor privilegio al requerir múltiples sujetos para tareas críticas.
- [[Complete Mediation]]: garantiza que los privilegios se verifiquen en cada acceso.
- [[PostgreSQL - Roles]]: implementación a nivel de base de datos.
- [[AWS IAM]]: implementación a nivel de infraestructura cloud.
- [[SELinux]]: implementación a nivel de sistema operativo.
