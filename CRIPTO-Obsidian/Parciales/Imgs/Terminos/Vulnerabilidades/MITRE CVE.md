**MITRE** es una organización creada por el gobierno de EEUU para centralizar la información relacionada con las vulnerabilidades de software.

## Función

- Cataloga vulnerabilidades conocidas en software y permite una **alerta temprana** para los responsables y usuarios de ese software.
- Actúa como registro público y autoritativo de vulnerabilidades.

## Formato del identificador

```
CVE-YYYY-NNNNN
```

| Campo | Descripción |
|---|---|
| `YYYY` | Año en que se descubrió la vulnerabilidad |
| `NNNNN` | Número de secuencia asignado |

## Ejemplos notables

| CVE | Sistema | Descripción |
|---|---|---|
| [[CVE-2021-44228]] | Log4j | Ejecución remota de código (_Log4Shell_) |
| [[CVE-2014-0160]] | OpenSSL | Filtración de memoria (_Heartbleed_) |

## Métricas

Cada CVE incluye métricas de severidad (CVSS score) que cuantifican el impacto. Ver: https://www.cve.org/About/Metrics

## Relación con otros conceptos

- [[CWE]]: catálogo complementario de debilidades estructurales que originan CVEs.
- [[Zero-Day]]: vulnerabilidades que aún no tienen CVE asignado.
- [[Amenazas y Vulnerabilidades]]: marco general.

## Referencia

https://www.cve.org/
