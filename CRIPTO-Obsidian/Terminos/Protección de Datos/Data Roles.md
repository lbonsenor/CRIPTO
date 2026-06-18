Los **data roles** definen las responsabilidades de las personas dentro de la organización respecto al uso y protección de los datos. Son un componente central de [[Data Governance]].

## Roles principales

| Rol | Descripción |
|---|---|
| **Data Owner** | Responsable del dato: define quién puede acceder, durante cuánto tiempo y para qué. Usualmente un rol de negocio (no técnico). |
| **Data Steward** | Custodio operativo del dato: asegura que se cumplan las políticas definidas por el Data Owner. |
| **Data Custodian** | Responsable técnico del almacenamiento y protección del dato (DBA, sysadmin). |
| **Data User** | Quien usa el dato en sus tareas habituales. Tiene los permisos mínimos necesarios ([[Least Privilege]]). |
| **Data Protection Officer (DPO)** | Rol requerido por regulaciones como [[GDPR]]. Responsable de asegurar el cumplimiento regulatorio en materia de privacidad. |

## Importancia

Definir roles claros evita ambigüedades sobre quién toma decisiones respecto a los datos, quién es responsable ante una brecha, y quién debe aprobar cambios en los permisos de acceso.

## Relación con otros conceptos

- [[Data Governance]]: los roles son el componente "personas" de la gobernanza.
- [[Control de Acceso]]: los roles determinan los permisos que se configuran.
- [[RBAC]]: el modelo de control de acceso que implementa estos roles técnicamente.
- [[Regulaciones de Protección de Datos]]: el GDPR exige la designación formal de un DPO.
