El **control por comprensión** define, por medio de **reglas**, cómo los principales (sujetos) pueden accionar sobre los objetos.

## Características
- Las reglas pueden definir acciones **permitidas** o **denegadas**.
- Las reglas pueden almacenarse **centralizadamente** o definirse **junto con los objetos**.
- Puede haber múltiples reglas que apliquen simultáneamente → se necesita una [[Resolución de Reglas]].

## Variantes según criterio de agrupación

| Variante | Descripción |
|---|---|
| [[RBAC]] | Control basado en **roles** |
| [[ABAC]] | Control basado en **atributos** |
| [[RuBAC]] | Control basado en **reglas** |

## Reglas contextuales
El control por comprensión puede extenderse con [[Reglas basadas en Contexto]], incorporando datos dinámicos del entorno (hora, ubicación, estado del sujeto) en las decisiones de acceso.

## Relación con otros controles

- [[Mandatory Access Control]]: aplica reglas obligatoriamente a todos los accesos.
- [[Discretionary Access Control]]: los permisos quedan a discreción del dueño del objeto.
- [[Control por Extensión]]: usa listas o matrices en lugar de reglas lógicas.
