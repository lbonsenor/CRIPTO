## El problema fundamental

El [[Control de Acceso]] limita el acceso a **operaciones sobre objetos**, pero **no impide que la información sea copiada** a otro lugar. Las políticas de seguridad en general restringen el **flujo de información**, no el acceso a objetos específicos.

Comparar:
- _Evitar que un empleado sepa el sueldo de otro_ (política de flujo)
- _Evitar que un empleado acceda a la base de datos de sueldos_ (control de acceso)

Los controles de acceso **sirven**, pero son mecanismos abiertos. Deben ser complementados con controles de flujo.

## El problema del confinamiento

Ver: [[Problema del Confinamiento]]

Si se pudiera confinar el flujo de información dentro de un ambiente controlado, no habría riesgos de confidencialidad o integridad. El [[Modelo Bell-La Padula]] define un modelo de seguridad para asegurar ese confinamiento.

## Canales no controlados

| Canal | Descripción | Archivo |
|---|---|---|
| **Covert Channel** | Comunicación oculta que evita controles de seguridad | [[Covert Channel]] |
| **Side-Channel Attack** | Extracción de información por efectos físicos del hardware | [[Side-Channel Attack]] |

## Control de flujo

| Momento | Mecanismo | Descripción |
|---|---|---|
| Compilación | Análisis estático, entropía | Analiza el código fuente directamente |
| Ejecución | Sandboxing, virtualización | Controla las interacciones del proceso |

Ver: [[Control de Flujo de Información]]

## Relación con otros conceptos

- [[Modelo Bell-La Padula]]: modelo de seguridad para el confinamiento del flujo.
- [[La triada]]: la confidencialidad es la dimensión principalmente afectada.
- [[Mandatory Access Control]]: implementa controles que limitan el flujo.
