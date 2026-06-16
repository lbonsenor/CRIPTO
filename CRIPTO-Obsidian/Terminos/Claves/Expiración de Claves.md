La **expiración de claves** es una política que fuerza a los usuarios a cambiar su contraseña luego de un tiempo determinado o tras ciertos eventos.

## Objetivo

Limitar el daño producido por una **clave comprometida**: aunque un atacante la obtenga, su ventana de uso es acotada.

## Requisitos de una buena política de expiración

- **Evitar el reuso**: recordar y bloquear las últimas N claves usadas.
- **Evitar el cambio acelerado**: impedir que el usuario cambie la clave varias veces en un período corto (para "quemar" el historial y volver a la clave original).
- **Dar tiempo al usuario**: no forzar el cambio en el momento del login.
  - Avisar con anticipación (ej: "Tu clave expira en 7 días").
  - Permitir el cambio en un momento conveniente.

## Consideraciones

- Una expiración demasiado frecuente lleva a los usuarios a elegir claves más débiles o predecibles (ej: `clave1`, `clave2`, `clave3`...).
- Debe combinarse con [[Revisión Proactiva de Claves]] para que las nuevas claves no sean triviales.

## Relación con otros conceptos

- [[Claves]]: objeto sobre el que se aplica la política.
- [[Revisión Proactiva de Claves]]: política complementaria.
- [[Ataques a Sistemas de Autenticación]]: la expiración limita la ventana de exposición ante un ataque exitoso.
