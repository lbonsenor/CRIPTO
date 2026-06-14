![[2-2025-Q1-2-EJ5.png]]
**Conceptos:**
- [[RBAC]]

Implementaremos una estructura jerárquica y funcional donde los permisos no se asignan a personas, sino a **roles** específicos dentro del flujo editorial y de investigación


| Rol          | Investigar | Crear/Editar | Aprobar | Publicar | Administrar |
| ------------ | ---------- | ------------ | ------- | -------- | ----------- |
| Investigador | X          |              |         |          |             |
| Editor       |            | X            |         |          |             |
| Aprobador    |            |              | X       |          |             |
| CM           |            |              |         | X        |             |
| IT           |            |              |         |          | X           |
