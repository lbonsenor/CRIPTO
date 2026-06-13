El **sandboxing** es una técnica de [[Aislamiento]] que **analiza y modifica las interacciones** de un proceso con su entorno para aislarlo, sin necesidad de emular hardware completo.

## Principio

En lugar de crear un entorno sintético completo ([[Virtualización]]), el sandboxing intercepta las llamadas del proceso al sistema operativo y las filtra, redirige o bloquea según una política definida.

## Implementaciones a nivel sistema operativo

### Linux

| Mecanismo | Descripción |
|---|---|
| **Linux Namespaces** | Aíslan procesos, usuarios, red, filesystem y más. Cada proceso ve solo los recursos de su namespace. |
| **cgroups** | Controlan y limitan el uso de recursos (CPU, memoria, red) para grupos de procesos. |

### Windows

| Mecanismo | Descripción |
|---|---|
| **User Mode Scheduling** | Similar a namespaces en Linux. |
| **Job Objects** | Gestionan grupos de procesos como unidad, asignando límites de recursos y prioridades. |

## Contenedores

Los **contenedores** son la forma más popular de sandboxing moderno. Aprovechan namespaces y cgroups (Linux) para aislar aplicaciones, permitiendo que múltiples contenedores compartan el mismo kernel del SO anfitrión manteniendo separación lógica en red, procesos y filesystem.

- **Docker** es el ejemplo más popular en Linux.
- Windows soporta contenedores nativos usando características propias o Hyper-V.

Ver: [[Contenedores]]

## Uso en navegadores

Los navegadores modernos como Chrome usan sandboxing para aislar el proceso de cada pestaña y plugin, limitando el daño en caso de explotación de una vulnerabilidad web.

Referencia: https://www.chromium.org/developers/design-documents/site-isolation/

## Relación con otros conceptos

- [[Aislamiento]]: el objetivo que el sandboxing implementa.
- [[Virtualización]]: técnica alternativa con aislamiento más fuerte pero mayor overhead.
- [[Contenedores]]: la aplicación práctica más difundida del sandboxing.
- [[Malware]]: el sandboxing se usa para analizar malware en entornos controlados.
- [[CWE-79 - XSS]]: el site isolation de Chrome mitiga este tipo de ataques.
