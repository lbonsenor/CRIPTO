**DAST** (_Dynamic Application Security Testing_) evalúa la seguridad de las aplicaciones **mientras se ejecutan**, interactuando con ellas como lo haría un usuario externo o un atacante.

## Características

- Utiliza las **interfaces estándar** de las aplicaciones (APIs, URLs, formularios web).
- No requiere acceso al código fuente.
- Identifica vulnerabilidades que solo se manifiestan en tiempo de ejecución: errores de validación de entrada, problemas de autenticación, configuraciones incorrectas del servidor, etc.
- Se centra en vulnerabilidades explotables por la red.

## Ejemplos de lo que detecta

- [[CWE-89 - SQL Injection]] a través de parámetros HTTP.
- [[CWE-79 - XSS]] en respuestas del servidor.
- Problemas de autenticación y gestión de sesiones.
- Exposición de datos sensibles en respuestas.

## Ventajas y limitaciones

| Ventaja | Limitación |
|---|---|
| Detecta vulnerabilidades reales en el entorno | Requiere que la aplicación esté desplegada |
| Pocos falsos positivos | No cubre todo el código (solo rutas alcanzables) |
| Simula ataques reales | Puede causar efectos en la aplicación analizada |

## Relación con otros conceptos

- [[DevSecOps]]: DAST se integra en las fases de build/test y deployment.
- [[SAST]]: complementario — analiza el código fuente estáticamente.
- [[SCA]]: complementario — analiza las dependencias.
- [[OWASP Top 10]]: referencia para los tipos de vulnerabilidades que detecta.
- [[PenTest]]: el DAST automatiza parte del trabajo de un pentest de aplicaciones web.
