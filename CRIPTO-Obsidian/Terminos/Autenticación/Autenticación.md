La **autenticación** es el proceso de asociar una identidad a un **principal**.

- La **identidad** corresponde a una entidad externa (usuario, sistema, servicio).
- El **principal** es la representación interna de esa entidad dentro del sistema.

```
Entorno            Sistema
────────           ────────
Usuario1  ──────►  Identidad
Usuario2  ──────►  autenticador  →  Principal
Sistema1  ──────►
Entidad externa
```

## Pasos de confirmación de identidad

1. **Obtener** información de identidad.
2. **Analizar** la información recibida.
3. **Determinar** si corresponde a la entidad que se declara.

### Consecuencias

- Hay que **almacenar** información de cada entidad.
- Hay que contar con **mecanismos** para procesar esa información.

## Componentes formales

Ver: [[Sistema de Autenticación]]

## Información de autenticación

La información puede provenir de distintas fuentes o **factores**:

| Factor | Descripción |
|---|---|
| [[Factor - Algo que conozco]] | Secretos compartidos (claves, frases) |
| [[Factor - Algo que tengo]] | Elementos físicos (token, celular) |
| [[Factor - Algo que soy]] | Biometría (huella, retina, cara) |
| [[Factor - Contexto]] | Datos del entorno (IP, geolocalización, hora) |

## Autenticación con múltiples factores

Ver: [[Multi-Factor Authentication]]

## Delegación de autenticación

Ver: [[SSO - Single Sign-On]]

## Relación con otros conceptos

- [[Identidad del Sujeto]]: lo que se busca verificar en la autenticación.
- [[Control de Acceso]]: la autenticación es prerrequisito del control de acceso.
- [[OAuth 2.0]]: estándar de autorización que complementa la autenticación.
- [[OpenID Connect]]: estándar de autenticación basado en OAuth 2.0.
