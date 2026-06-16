**SAST** (_Static Application Security Testing_) analiza la seguridad del código fuente **sin ejecutarlo**, buscando patrones que indiquen vulnerabilidades.

## Características

- Puede ejecutarse en la fase de desarrollo, antes de que el código sea compilado o desplegado.
- Detecta problemas como [[CWE-89 - SQL Injection|inyecciones]], [[CWE-79 - XSS|XSS]], uso de funciones inseguras, secretos hardcodeados, etc.
- Integrable en el IDE del desarrollador o en el pipeline CI/CD (pre-commit, on PR).

## Ventajas y limitaciones

| Ventaja | Limitación |
|---|---|
| Feedback temprano (Shift Left) | No puede detectar vulnerabilidades que emergen en tiempo de ejecución |
| Cubre el 100% del código | Alta tasa de falsos positivos |
| No requiere desplegar la aplicación | No entiende el contexto de ejecución |

## Relación con otros conceptos

- [[DevSecOps]]: SAST se integra en las fases de desarrollo y commit.
- [[DAST]]: complementario — analiza la aplicación en ejecución.
- [[CWE]]: los tipos de defectos que SAST busca detectar.
