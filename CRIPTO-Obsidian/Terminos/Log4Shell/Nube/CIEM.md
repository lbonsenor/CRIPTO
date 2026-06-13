**CIEM** (_Cloud Infrastructure Entitlement Management_) se focaliza en los **permisos de los sujetos sobre los recursos de la nube**, con el objetivo de garantizar el [[Least Privilege]] en entornos cloud.

## Funciones principales

- **Detectar permisos excesivos**: identifica sujetos (usuarios, roles, servicios) con más permisos de los que realmente usan.
- **Comparar uso real vs. permisos otorgados**: cruza los registros de uso de accesos contra los permisos configurados para identificar y recomendar la remoción de permisos no utilizados.
- **Visualización de permisos**: muestra de forma clara los permisos y su consistencia entre cuentas cloud (multi-cloud).
- **Consistencia entre cuentas**: permite auditar permisos en múltiples cuentas o proveedores simultáneamente.

## Problema que resuelve

En entornos cloud, los permisos tienden a acumularse con el tiempo (_permission sprawl_): se otorgan permisos para una tarea puntual y nunca se revocan. CIEM automatiza la detección de esta acumulación.

## Diferencia con CSPM

| Aspecto | [[CSPM]] | CIEM |
|---|---|---|
| Foco | Configuración general del entorno | Permisos e identidades |
| Qué busca | Malas configuraciones | Permisos excesivos o no usados |

## Relación con otros conceptos

- [[Least Privilege]]: el principio que CIEM implementa y verifica.
- [[AWS IAM]]: uno de los sistemas de permisos que CIEM audita.
- [[CSPM]]: complemento de enfoque en configuración.
- [[CWPP]]: complemento de enfoque en workloads en ejecución.
- [[CNAPP]]: plataforma que integra CSPM, CWPP y CIEM.
- [[RBAC]]: CIEM audita la efectividad de los roles definidos.

## Referencia

https://www.zenarmor.com/docs/network-security-tutorials/what-is-cloud-infrastructure-entitlement-management-ciem
