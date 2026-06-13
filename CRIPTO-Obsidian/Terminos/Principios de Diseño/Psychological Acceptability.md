> Principio 8 de [[Principios de Diseño de Seguridad]]

**Menor asombro** establece que los controles de seguridad deben ser **simples de usar** y no deben complicar el negocio o los procesos del usuario.

## Fundamento

- El **peor enemigo** de cualquier diseño de seguridad es el usuario **no alineado** con él.
- El **mejor aliado** es el usuario **comprometido** con la seguridad.

Un control que resulta engorroso o incomprensible será sistemáticamente evadido o ignorado por los usuarios, incluso con buenas intenciones.

## Implicaciones prácticas

- Los controles deben ser usables **inclusive para personas sin conocimientos técnicos**.
- La **concientización** sobre el problema y el valor de la seguridad es fundamental para lograr la aceptación del control.
- El nombre "menor asombro" refiere a que el sistema debe comportarse de la forma que el usuario **espera intuitivamente**, sin sorpresas que lo lleven a cometer errores.

## Ejemplos de violación de este principio

- Forzar cambios de contraseña tan frecuentes que los usuarios empiezan a usar `clave1`, `clave2`, etc.
- Un proceso de aprobación tan complejo que los empleados buscan atajos para evitarlo.
- Mensajes de error crípticos que llevan a los usuarios a deshabilitar controles.

## Relación con otros conceptos

- [[Expiración de Claves]]: si la política es demasiado estricta, viola este principio.
- [[Multi-Factor Authentication]]: debe implementarse de forma que no genere fricción excesiva.
- [[Economy of Mechanism]]: la simplicidad técnica favorece la aceptación del usuario.
