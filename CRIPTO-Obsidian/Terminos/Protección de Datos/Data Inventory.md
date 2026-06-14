# Data Inventory

El **data inventory** (inventario de datos) identifica y registra todos los **datos críticos** de la organización: su ubicación, uso, [[Clasificación de Datos|clasificación]] y los responsables ([[Data Roles]]).

## Función

- Sirve de **guía de referencia** para la toma de decisiones, el diseño de la seguridad y la aplicación de políticas.
- Es la base para saber qué proteger, dónde está, quién accede y bajo qué condiciones.

## Desafíos

| Desafío | Descripción |
|---|---|
| **Costo de mantenimiento** | Mantenerlo actualizado es costoso en tiempo y recursos. |
| **Tipos de datos** | Diversidad de formatos, sistemas y fuentes de datos. |
| **Flujos de datos** | Los datos se mueven entre sistemas, dificultando el seguimiento. |
| **Costo de inventariar** | Descubrir nuevos tipos de datos requiere esfuerzo continuo. |
| **Falta de uso** | Sin educación, el inventario existe pero no se consulta. |
| **Sensibilidad del inventario** | El inventario mismo es un activo crítico: contiene info sobre dónde están los datos más sensibles. Debe protegerse bajo las mismas reglas. |
| **Dinámica empresarial** | En empresas ágiles, el inventario queda desactualizado rápidamente. |

## Alternativa: Data Labeling

Para mitigar los desafíos de mantenimiento centralizado, existe el modelo [[Data Labeling]]: embeber el inventario **dentro de los propios datos** como metadata.

## Relación con otros conceptos

- [[Data Governance]]: el inventario es el componente central de la fase _Discover_.
- [[Clasificación de Datos]]: la clasificación se registra en el inventario.
- [[Data Labeling]]: alternativa descentralizada al inventario centralizado.
- [[DLP - Data Loss Prevention]]: las herramientas DLP se alimentan del inventario para saber qué detectar.

## Referencia

https://blog.soteria.io/data-inventory-and-classification-75d255c57b10
