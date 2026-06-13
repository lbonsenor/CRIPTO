La **revisión proactiva de claves** consiste en analizar una clave **al momento de su creación** para detectar y rechazar claves débiles antes de que sean usadas.

## Objetivos

- Evitar que los usuarios elijan claves fácilmente adivinables.
- Reducir la superficie de ataque ante ataques de diccionario o fuerza bruta.

## Capacidades recomendadas

El sistema debería poder:

- Aplicar **reglas sobre palabras** (ej: no permitir el nombre del usuario, fecha de nacimiento).
- Realizar **búsquedas en diccionarios** de palabras comunes y claves comprometidas conocidas.
- Detectar **patrones simples** (ej: `12345678`, `aaaaaaaa`, `qwerty`).
- Utilizar **información de contexto** del usuario (nombre, usuario, DNI, etc.) para descartar derivaciones obvias.

## Relación con [[Ataques a Sistemas de Autenticación]]

Contrarresta directamente los ataques de diccionario y transformaciones simples, ya que impide que esas claves existan en el sistema.

## Relación con otros conceptos

- [[Claves]]: objeto sobre el que se aplica la revisión.
- [[Expiración de Claves]]: política complementaria de gestión de claves.
- [[Fórmula de Anderson]]: permite estimar si una política de complejidad es suficiente.
