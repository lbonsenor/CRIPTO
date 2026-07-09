Formato de token compuesto por tres partes codificadas en base64 y separadas por puntos: header, payload (claims) y firma/verificación. A diferencia de una cookie de sesión tradicional, el servidor no necesita guardar estado — toda la información relevante viaja en el propio token.

Riesgos comunes que se evalúan en ejercicios de pentesting:
- **Sin campo `exp`**: el token nunca expira, por lo que un token robado sirve indefinidamente (mismo problema que resuelve [[Denning-Sacco]]).
- **Guardado en localStorage**: queda expuesto a robo vía [[CWE-79 - XSS]], ya que cualquier script en la página puede leerlo.
- **Decodificable sin la clave**: el payload de un JWT no está cifrado, solo codificado en base64 — cualquiera puede leer su contenido (aunque no pueda falsificar la firma sin la clave).

Por estar en un header de autorización (no en una cookie), un JWT normalmente no es vulnerable a [[CSRF - Cross-Site Request Forgery]], a diferencia de la autenticación basada en cookies.
