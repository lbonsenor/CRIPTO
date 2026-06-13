Los **contenedores** son la forma más difundida de [[Sandboxing]] moderno. Permiten aislar aplicaciones usando primitivas del sistema operativo, sin emular hardware completo como en [[Virtualización]].

## Mecanismo (Linux)

Aprovechan dos tecnologías del kernel Linux:

- **Namespaces**: aislan la vista de procesos, red, usuarios y filesystem de cada contenedor.
- **cgroups**: limitan los recursos (CPU, memoria, red) disponibles para cada contenedor.

El resultado es que múltiples contenedores comparten el **mismo kernel** del SO anfitrión, pero mantienen **separación lógica** completa entre sí.

## Docker

**Docker** es el runtime de contenedores más popular. Empaqueta una aplicación junto con sus dependencias en una imagen portable que puede ejecutarse de forma consistente en cualquier entorno.

## Soporte en Windows

Windows soporta dos modos de contenedores:

| Modo | Descripción |
|---|---|
| **Windows Containers** | Usan características nativas de Windows. Comparten el kernel de Windows. |
| **Hyper-V Containers** | Cada contenedor corre en una VM Hyper-V mínima. Mayor aislamiento. |

## Comparación con VMs

| Aspecto | Contenedores | [[Virtualización]] (VM) |
|---|---|---|
| Kernel | Compartido | Propio |
| Overhead | Bajo | Alto |
| Arranque | Milisegundos | Segundos / minutos |
| Aislamiento | Moderado | Fuerte |
| Portabilidad | Alta | Media |

## Relación con otros conceptos

- [[Sandboxing]]: el paradigma del que los contenedores son la implementación más popular.
- [[Virtualización]]: alternativa con mayor aislamiento.
- [[Aislamiento]]: el objetivo de seguridad que implementan.
- [[Least Privilege]]: los contenedores facilitan aplicar mínimos privilegios por servicio.
