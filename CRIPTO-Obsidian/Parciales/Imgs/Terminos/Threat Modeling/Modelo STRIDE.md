**STRIDE** es un framework desarrollado por **Microsoft** para identificar sistemáticamente las amenazas en un sistema. Es uno de los frameworks más utilizados para [[Threat Modeling]].

## Categorías de amenazas

Cada letra de STRIDE representa un tipo de amenaza:

| Letra | Amenaza | Descripción | Propiedad violada |
|---|---|---|---|
| **S** | _Spoofing_ | Suplantar la identidad de un usuario o sistema | [[Autenticación]] |
| **T** | _Tampering_ | Modificar datos sin autorización | Integridad |
| **R** | _Repudiation_ | Negar haber realizado una acción | No-repudio |
| **I** | _Information Disclosure_ | Exponer información a quien no debería tenerla | Confidencialidad |
| **D** | _Denial of Service_ | Interrumpir la disponibilidad del sistema | Disponibilidad |
| **E** | _Elevation of Privilege_ | Obtener permisos mayores a los asignados | Autorización |

## Uso con DFD

STRIDE se utiliza en conjunto con el [[Data Flow Model (DFD)]]:

1. Se construye el DFD del sistema.
2. Se cruza cada elemento del DFD con las categorías de STRIDE.
3. Se identifican los riesgos resultantes.
4. Se definen contramedidas para cada riesgo.

## Relación con los principios de diseño

| Amenaza STRIDE | Principio de diseño mitigante |
|---|---|
| Spoofing | [[Autenticación]], [[Multi-Factor Authentication]] |
| Tampering | [[Firma Digital]], [[Message Authentication Code]] |
| Repudiation | Logging, [[Firma Digital]] |
| Information Disclosure | [[Least Privilege]], cifrado |
| Denial of Service | [[Defense in Depth]], rate limiting |
| Elevation of Privilege | [[Least Privilege]], [[Separation of Privilege]] |

## Referencia

https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats
