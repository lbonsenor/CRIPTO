El **aislamiento** es el conjunto de técnicas de [[Control de Flujo de Información]] en tiempo de ejecución que impiden que un proceso acceda o transmita información fuera de los límites definidos.

## Enfoques

Los sistemas aíslan procesos de dos maneras complementarias:

| Técnica | Mecanismo | Archivo |
|---|---|---|
| **Virtualización** | Ambiente de ejecución sintético, separado del real y aislado por construcción | [[Virtualización]] |
| **Sandboxing** | Control y modificación de las interacciones del proceso con su entorno | [[Sandboxing]] |

## Relación con el confinamiento

El aislamiento es la implementación práctica del [[Problema del Confinamiento]]: en lugar de confinamiento perfecto (imposible en la práctica), se construyen **barreras controladas** que limitan el flujo de información a canales autorizados.

## Relación con otros conceptos

- [[Control de Flujo de Información]]: el aislamiento es su implementación en tiempo de ejecución.
- [[Covert Channel]]: los canales encubiertos son formas de romper el aislamiento.
- [[Malware]]: el aislamiento es una de las defensas primarias contra la propagación de malware.
- [[Sandboxing]]: técnica basada en interceptar llamadas al sistema.
- [[Virtualización]]: técnica basada en emulación de hardware.
