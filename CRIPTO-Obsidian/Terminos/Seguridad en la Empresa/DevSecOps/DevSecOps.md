**DevSecOps** es la integración de la seguridad en cada etapa del ciclo de desarrollo de software (_Shift Left_): en lugar de agregar seguridad al final, se la construye desde el inicio.

## Principio: Shift Left

"Shift Left" implica mover los controles de seguridad lo más temprano posible en el ciclo de desarrollo, donde los errores son más baratos de corregir.

## Fases y controles

### Planeamiento y desarrollo

- Aplicar [[Principios de Diseño de Seguridad]] en el diseño.
- Usar asistentes de AI para el desarrollo de código seguro.
- **Pre-commit hooks**: forzar controles automáticos antes de hacer un commit.
- Aplicar mejores prácticas de desarrollo seguro y entrenamiento continuo.
- **Code review / PR review**: revisión de vulnerabilidades y correcciones.

### On Commit / PR

- **Unit tests de seguridad**: foco en [[CWE|CWEs]] específicos.
- **[[SCA]]**: control de vulnerabilidades en dependencias.
- Protección de pipelines: resguardar acceso a recursos confidenciales y claves de seguridad.

### On Build / Test

- **[[DAST]]**: controles dinámicos ejecutando el código contra sus interfaces (APIs, endpoints).
- Validación de [[IaC]] (Infraestructura como Código) antes y después del despliegue en la nube.
- Evaluación de seguridad de la aplicación como un todo.

### On Deployment

- Evaluación de seguridad incluyendo el ambiente de despliegue (Firewalls, WAF, IPS).
- Controles sobre artefactos consumidos durante el despliegue (configuraciones, secretos).
- **[[PenTest]]** externo.
- [[Red Blue y Purple Team|Red/Blue Team]].

### On Operation

- Monitoreo continuo de métricas y logs de seguridad. Ver [[SIEM]].
- Integración con **Threat Intelligence**.
- **Blind Penetration Tests**.
- Ejercicios de simulación de incidentes ([[BAS]]).
- Respuesta ante incidentes.

## Herramientas de testing de seguridad

| Herramienta | Descripción | Archivo |
|---|---|---|
| **SAST** | Análisis estático del código fuente | [[SAST]] |
| **DAST** | Análisis dinámico de la aplicación en ejecución | [[DAST]] |
| **SCA** | Análisis de dependencias y bibliotecas | [[SCA]] |

## Inventario de software

Ver [[SBOM]] (_Software Bill of Materials_): inventario formal de componentes y dependencias.

## Referencia

https://www.netsolutions.com/insights/what-is-devsecops/

## Relación con otros conceptos

- [[Principios de Diseño de Seguridad]]: los principios que se aplican desde el inicio.
- [[Threat Modeling]]: actividad de la fase de planeamiento.
- [[CWE]]: los defectos que los tests de seguridad buscan detectar.
- [[OWASP Top 10]]: referencia para los tests de aplicaciones web.
