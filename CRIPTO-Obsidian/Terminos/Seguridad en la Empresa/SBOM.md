Un **SBOM** (_Software Bill of Materials_) es un **inventario formal y legible por máquina** de todos los componentes de software, sus dependencias y las relaciones entre ellos.

## Contenido

- Componentes de código abierto y con licencia comercial.
- Versiones exactas de cada componente.
- Relaciones entre componentes (dependencias directas y transitivas).
- Información sobre licencias.
- Indicación explícita de dónde no pueden articularse relaciones (opacidad reconocida).

## Propiedades

- Debe ser lo más **completo** posible.
- Puede estar ampliamente disponible o tener **acceso restringido** para proteger información confidencial o de propiedad exclusiva.
- Es legible por máquina, lo que permite su integración en pipelines automáticos.

## Uso en seguridad

El SBOM permite:

- Saber **instantáneamente** qué sistemas se ven afectados cuando se descubre una nueva vulnerabilidad (como [[CVE-2021-44228]]).
- Ejecutar [[SCA]] sobre un inventario formalizado.
- Cumplir con regulaciones que exigen transparencia en el software (ej: órdenes ejecutivas de ciberseguridad de EEUU).

## Lugar en el ciclo DevSecOps

Se genera y actualiza en las fases **On Build** y **On Deployment** del ciclo [[DevSecOps]].

## Relación con otros conceptos

- [[SCA]]: la herramienta que analiza las dependencias y puede generar el SBOM.
- [[DevSecOps]]: el proceso en el que se integra.
- [[MITRE CVE]]: el catálogo contra el que se cruza el inventario del SBOM.
- [[Cloud Vulnerabilities]]: los SBOMs son especialmente relevantes en entornos cloud-native con muchas dependencias.

## Referencia

https://www.cisa.gov/sites/default/files/2024-10/SBOM%20Framing%20Software%20Component%20Transparency%202024.pdf
