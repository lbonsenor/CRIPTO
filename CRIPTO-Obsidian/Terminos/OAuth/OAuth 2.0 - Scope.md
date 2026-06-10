El **scope** (alcance) en [[OAuth 2.0]] define los **permisos específicos** que una aplicación solicita al usuario para acceder a sus datos.

## Características

- Los scopes definen el **nivel de acceso** que la aplicación tendrá.
- Se presentan al usuario en la **pantalla de consentimiento** durante el flujo de autorización.
- El **access token** emitido queda limitado a los scopes otorgados por el usuario.
- Cada servicio que recibe el token **verifica que contenga su propio scope** en la lista de scopes autorizados.

## Ejemplos de scopes

| Scope | Acceso otorgado |
|---|---|
| `email` | Solo la dirección de correo del usuario |
| `contacts:read` | Lectura de contactos |
| `contacts:write` | Modificación de contactos |
| `profile` | Información básica del perfil |

## Relación con DAC

El mecanismo de scopes es una implementación de [[Discretionary Access Control]]: es el **usuario quien decide** qué permisos otorgar a la aplicación al momento de dar su consentimiento.

- Una aplicación puede solicitar uno o más scopes.
- El usuario puede **aprobar o denegar** cada scope individualmente.

## Relación con otros conceptos

- [[OAuth 2.0]]: el mecanismo del que forman parte los scopes.
- [[Discretionary Access Control]]: el usuario controla los permisos otorgados.
- [[OAuth 2.0 - Token Refresh]]: el refresh token no puede ampliar los scopes originales.
