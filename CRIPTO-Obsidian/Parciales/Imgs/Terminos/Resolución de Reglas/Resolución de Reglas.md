Cuando un sistema de [[RuBAC]] posee muchas reglas que aplican en forma **contradictoria**, se utiliza alguna de las siguientes políticas de resolución:

| Política | Descripción |
|---|---|
| [[First Match]] | La primera regla en orden que hace match se aplica |
| [[Best Match]] | La regla más específica es la que aplica |
| [[Deny over Allow]] | Cualquier Deny explícito tiene precedencia sobre los Allow |

## ¿Cuándo se usa cada una?

- **[[First Match]]**: usado en firewalls, [[Squid Web Proxy]], iptables. El orden de las reglas importa.
- **[[Best Match]]**: usado cuando las reglas pueden solaparse y la más específica refleja mejor la intención.
- **[[Deny over Allow]]**: usado en [[AWS IAM]], [[Windows File System]]. La seguridad prima sobre la permisividad.

## Relación con otros conceptos

- [[RuBAC]]: el contexto donde surge la necesidad de resolver reglas conflictivas.
- [[Control por Comprensión]]: la resolución es parte de su funcionamiento.
