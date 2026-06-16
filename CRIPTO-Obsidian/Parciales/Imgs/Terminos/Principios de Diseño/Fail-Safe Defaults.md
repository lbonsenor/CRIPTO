> Principio 2 de [[Principios de Diseño de Seguridad]]

**Fallar de forma segura** establece que, ante una falla o situación no contemplada, el sistema debe moverse a un **estado seguro** (denegación por defecto).

## Reglas derivadas

- Las **listas de permitidos** (_whitelists_) son mejores que las **listas de excluidos** (_blacklists_): lo que no está explícitamente permitido, está denegado.
- Un sistema **no debe abrir puertos innecesariamente**.
- No debe haber **cuentas de usuario por defecto** con contraseñas conocidas.

## Ejemplo

Si el sistema de [[Autenticación]] no puede conectarse a la base de datos para verificar una contraseña, debe **rechazar el pedido** y no asignar permisos, aunque sean los mínimos posibles.

> Permitir el acceso ante incertidumbre viola este principio.

## Relación con otros conceptos

- [[Deny over Allow]]: política de resolución de reglas que implementa este principio.
- [[Blacklist]]: el enfoque menos seguro, opuesto a la whitelist.
- [[Least Privilege]]: complemento natural — si falla, no se acumula privilegio.
- [[Defense in Depth]]: los controles compensatorios también deben fallar de forma segura.
