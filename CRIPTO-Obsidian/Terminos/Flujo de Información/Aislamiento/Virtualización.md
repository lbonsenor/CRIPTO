La **virtualización** es una técnica de [[Aislamiento]] que crea un **ambiente de ejecución sintético** separado del entorno real, aislado por construcción mediante un _hipervisor_.

## Máquinas Virtuales (VMs)

Las VMs usan **hipervisores** para emular hardware y gestionar distintos sistemas operativos en un solo host físico.

### Tipos de hipervisores

| Tipo | Descripción | Ejemplos |
|---|---|---|
| **Bare-metal** (Tipo 1) | Corre directamente sobre el hardware, sin SO anfitrión | KVM, VMware ESXi, Hyper-V |
| **Hosted** (Tipo 2) | Corre sobre un SO anfitrión existente | VirtualBox, VMware Workstation |

### Propiedades de aislamiento

- Cada VM tiene su **propio kernel**, lo que proporciona **fuerte aislamiento** entre ellas.
- Un proceso dentro de una VM no puede acceder directamente a recursos de otra VM ni del host.
- Aislamiento más fuerte que el de los [[Sandboxing|contenedores]], a costa de mayor overhead.

## Comparación con Sandboxing

| Aspecto | Virtualización (VM) | [[Sandboxing]] / Contenedores |
|---|---|---|
| Kernel | Propio por VM | Compartido con el host |
| Aislamiento | Más fuerte | Más ligero |
| Overhead | Alto | Bajo |
| Arranque | Lento (segundos/minutos) | Rápido (milisegundos) |

## Relación con otros conceptos

- [[Aislamiento]]: el objetivo que la virtualización implementa.
- [[Sandboxing]]: técnica complementaria de aislamiento más liviana.
- [[Problema del Confinamiento]]: la virtualización es una solución práctica al confinamiento.
- [[Malware]]: las VMs se usan para ejecutar malware en entornos controlados (sandboxes de análisis).
