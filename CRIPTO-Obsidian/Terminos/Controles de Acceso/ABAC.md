****Attribute-Based Access Control (ABAC)** es una variante del [[Control por Comprensión]] donde los permisos se definen en función de **atributos** del sujeto, el objeto, o el entorno.

## Características

- Los atributos pueden pertenecer al **sujeto** (departamento, cargo, nivel de clearance), al **objeto** (clasificación, propietario, tipo) o al **entorno** (hora, ubicación).
- Ofrece mayor **granularidad** que [[RBAC]], al no limitarse a roles fijos.
- Es la base natural de las [[Reglas basadas en Contexto]].

## Ejemplo

> "Un usuario del departamento de Finanzas puede leer documentos clasificados como 'Financiero' solo durante el horario laboral."

## Ventajas y desventajas

| Ventaja | Desventaja |
|---|---|
| Muy flexible y expresivo | Mayor complejidad de gestión |
| Permite reglas contextuales | Difícil de auditar con muchas reglas |
| Escala bien en entornos dinámicos | Requiere infraestructura de atributos |

## Relación con otros modelos

- [[RBAC]]: más simple, sólo usa roles.
- [[RuBAC]]: usa reglas explícitas, puede solaparse con ABAC.
- [[Open Policy Agent]]: permite implementar políticas ABAC complejas usando [[Rego]].
