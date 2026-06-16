**OAuth 2.0** es el estándar de autorización ampliamente utilizado para permitir que las aplicaciones accedan a recursos en una plataforma **en nombre del usuario**, sin que este tenga que compartir sus credenciales con la aplicación.

## Características principales

- Emite **tokens de acceso temporales y específicos** que permiten el acceso a los recursos del usuario.
- Es la versión más moderna de OAuth (sucesor de OAuth 1.0), con mayor flexibilidad y seguridad.
- Implementa un modelo de [[Discretionary Access Control]]: el usuario consiente qué permisos otorgar a la aplicación.

## Flujo básico

```
Usuario ──── Autoriza ────► Servidor de Autorización
                                    │
                             Access Token
                                    │
Aplicación ──── Token ────► Servidor de Recursos
                                    │
                              Datos del usuario
```

## Token Refresh

Cuando el _access token_ expira, la aplicación puede usar un **refresh token** para obtener un nuevo access token sin requerir que el usuario vuelva a autenticarse.

Ver: [[OAuth 2.0 - Token Refresh]]

## Scopes

Los **scopes** definen el nivel de acceso que la aplicación tendrá sobre los recursos del usuario.

Ver: [[OAuth 2.0 - Scope]]

## Relación con otros conceptos

- [[Discretionary Access Control]]: el usuario decide qué permisos otorgar.
- [[API Gateway]]: valida tokens OAuth como parte del control de acceso.
- [[OAuth 2.0 - Scope]]: mecanismo de granularidad de permisos.
- [[Identidad del Sujeto]]: OAuth identifica al sujeto mediante tokens.

## Referencia

https://goteleport.com/blog/how-oauth-authentication-works/
