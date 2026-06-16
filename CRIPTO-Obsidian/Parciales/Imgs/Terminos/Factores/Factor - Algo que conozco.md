El factor **"algo que conozco"** basa la identificación en **secretos compartidos** entre la entidad y el sistema.

## Ejemplos

- Contraseña / clave
- Frase clave (_passphrase_)
- Pregunta secreta

## Características

- Es uno de los mecanismos de [[Autenticación]] **más empleados**.
- Sirve de base a otros mecanismos más complejos.
- **Requiere almacenar de manera segura el secreto** → ver [[Claves]], [[PBKDF2]].
- La seguridad **depende de la confidencialidad** del secreto: si se compromete, la autenticación falla.

## Debilidades

- Los usuarios tienden a elegir claves predecibles → ver [[Ataques a Sistemas de Autenticación]].
- Vulnerable a ataques de diccionario y fuerza bruta.
- Si el canal no es seguro, la clave puede ser interceptada → ver [[Challenge-Response]], [[EKE]].

## Relación con otros conceptos

- [[Claves]]: implementación concreta de este factor.
- [[Factor de Autenticación]]: categoría a la que pertenece.
- [[Multi-Factor Authentication]]: se combina con otros factores para mayor seguridad.
- [[Challenge-Response]]: evita transmitir el secreto directamente.
