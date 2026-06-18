Los controles de **enforcement** garantizan que los datos no fluyan de manera no autorizada entre componentes del sistema. Se aplican en dos momentos distintos.

## Compiler-time (tiempo de compilación)

Asegura que el código cumple con políticas de seguridad **antes de ejecutarse**, mediante análisis estático del código fuente.

- El compilador analiza cómo fluye la información entre variables.
- Detecta si datos clasificados como confidenciales podrían filtrarse a variables de menor seguridad.
- Ejemplo: **SecureString** de Java — tipo de dato que el compilador trata especialmente para evitar que el valor sea copiado a strings ordinarios o expuesto en logs.

Este enfoque es el equivalente del [[Control de Flujo de Información|análisis de flujo en tiempo de compilación]] aplicado a la protección de datos.

## Execution-time (tiempo de ejecución)

Es la técnica **más utilizada** para monitorear y controlar cómo se transmite la información entre componentes.

### Enfoques

| Nivel | Descripción |
|---|---|
| **Aplicativo** | Lógica dentro de la aplicación que verifica si una transferencia de datos está permitida antes de ejecutarla. |
| **Infraestructura (DLP)** | Herramientas externas que monitorean el movimiento de datos independientemente de la aplicación. |

### Requisitos

- Entender el movimiento de datos dentro del sistema de información.
- Conocer los flujos entre procesos, entre niveles de seguridad y entre distintos estados del dato.

Ver: [[DLP - Data Loss Prevention]]

## Relación con otros conceptos

- [[Control de Flujo de Información]]: el marco teórico del que este enforcement es la implementación práctica.
- [[DLP - Data Loss Prevention]]: la herramienta que implementa el enforcement en tiempo de ejecución.
- [[SAST]]: análogo al compiler-time enforcement a nivel de vulnerabilidades de código.
- [[Estados de los Datos]]: el enforcement aplica a los tres estados (at-rest, in-motion, in-use).
- [[Clasificación de Datos]]: la clasificación define qué flujos son permitidos y cuáles no.
