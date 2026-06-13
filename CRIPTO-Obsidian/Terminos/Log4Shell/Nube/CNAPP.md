**CNAPP** (_Cloud-Native Application Protection Platform_) es una plataforma que integra las distintas herramientas de protección cloud en una solución unificada.

## Componentes integrados

| Herramienta | Foco | Archivo |
|---|---|---|
| **[[CSPM]]** | Configuración e IaC — enfoque estático | [[CSPM]] |
| **[[CWPP]]** | Workloads en ejecución — enfoque dinámico | [[CWPP]] |
| **[[CIEM]]** | Permisos e identidades | [[CIEM]] |

## Por qué es necesaria la integración

Cada herramienta por separado tiene un ángulo ciego:
- CSPM no ve lo que pasa en ejecución.
- CWPP no audita la configuración base ni los permisos.
- CIEM no analiza el comportamiento de las aplicaciones.

CNAPP combina las tres perspectivas para una cobertura completa del entorno cloud-native.

## Contexto de uso

El uso de CNAPP responde a los desafíos de [[Defense in Depth]] en entornos distribuidos:
- Data centers híbridos (propios y de terceros).
- Múltiples proveedores de nube (multi-cloud).
- Empresas cloud-native.

## Relación con otros conceptos

- [[Cloud Vulnerabilities]]: el conjunto de riesgos que CNAPP aborda.
- [[Zero Trust Architecture]]: CNAPP es una implementación práctica de ZTA en la nube.
- [[Gobernanza]]: CNAPP provee visibilidad y control para la gobernanza cloud.

## Referencia

https://www.gartner.com/doc/reprints?id=1-2I4YAW54&ct=240723&st=sb
