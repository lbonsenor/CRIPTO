**SCA** (_Software Composition Analysis_) analiza las **dependencias y bibliotecas externas** utilizadas en una aplicación para identificar vulnerabilidades de seguridad, problemas de licencia y otros riesgos asociados a esos componentes.

## Qué analiza

- Librerías de terceros incluidas en el proyecto (npm, pip, Maven, etc.).
- Versiones de bases de datos, APIs o ambientes de ejecución cloud usados como servicios SaaS.
- Relaciones entre dependencias transitivas (dependencias de dependencias).

## Por qué es crítico

La mayoría de las aplicaciones modernas incorporan más código de terceros que código propio. Una vulnerabilidad en una dependencia afecta a todos los proyectos que la usan — como demostró [[CVE-2021-44228]] (Log4Shell), donde una única librería comprometió miles de sistemas.

## Diferencias con SAST y DAST

| Herramienta | Qué analiza | Cuándo |
|---|---|---|
| [[SAST]] | Código propio, estáticamente | Pre-commit / On Commit |
| [[DAST]] | Aplicación en ejecución, dinámicamente | On Build / On Deployment |
| **SCA** | Dependencias y librerías externas | On Commit / On Build |

## Lugar en el ciclo DevSecOps

Se aplica principalmente en las fases **On Commit/PR** y **On Build**, como parte de los controles automatizados de [[DevSecOps]].

## Complemento: SBOM

El [[SBOM]] es el inventario formal de componentes que el SCA debería poder generar y mantener actualizado.

## Relación con otros conceptos

- [[DevSecOps]]: el proceso en el que se integra.
- [[SBOM]]: el inventario que SCA ayuda a construir.
- [[MITRE CVE]]: las vulnerabilidades conocidas que SCA detecta en dependencias.
- [[CWE]]: los tipos de debilidades que SCA busca en librerías.

## Referencia

https://medium.com/@reach2shristi.81/security-testing-tools-sast-dast-sca-c42ffc9be144
