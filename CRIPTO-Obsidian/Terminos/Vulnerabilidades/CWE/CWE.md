**CWE** es un registro de las **debilidades en software** que dan lugar a que las vulnerabilidades sucedan.

## Diferencia con CVE

| Concepto | Descripción |
|---|---|
| [[MITRE CVE]] | Instancia concreta de vulnerabilidad en un producto específico |
| CWE | Tipo de debilidad estructural en el código que origina vulnerabilidades |

Una CWE puede originar muchos CVEs distintos en distintos sistemas.

## Top 25 anual

Todos los años MITRE publica la lista de las **debilidades más comunes** en las vulnerabilidades descubiertas ese año.

https://cwe.mitre.org/top25/archive/2023/2023_top25_list.html

## CWEs destacadas en la clase

| CWE | Nombre | Archivo |
|---|---|---|
| CWE-787 | Out-of-bounds Write (Buffer Overflow) | [[CWE-787 - Buffer Overflow]] |
| CWE-79 | Cross-Site Scripting (XSS) | [[CWE-79 - XSS]] |
| CWE-89 | SQL Injection | [[CWE-89 - SQL Injection]] |
| CWE-416 | Use After Free | [[CWE-416 - Use After Free]] |
| CWE-78 | OS Command Injection | [[CWE-78 - OS Command Injection]] |

## Relación con otros conceptos

- [[MITRE CVE]]: cada CVE está asociado a una o más CWEs.
- [[OWASP Top 10]]: lista de vulnerabilidades web que mapea sobre CWEs.
- [[Amenazas y Vulnerabilidades]]: marco general.
