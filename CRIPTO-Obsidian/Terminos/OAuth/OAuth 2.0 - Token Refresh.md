El **token refresh** es el mecanismo de [[OAuth 2.0]] que permite obtener un nuevo _access token_ cuando el anterior ha expirado, sin requerir que el usuario vuelva a autenticarse.

## Flujo de refresco

```
Aplicación ──── Refresh Token ────► Servidor de Autorización
                                           │
                                    Nuevo Access Token
                                           │
Aplicación ──── Nuevo Token ────► Servidor de Recursos
```

## Por qué existe

Los **access tokens son de corta duración** por razones de seguridad: si son interceptados, su ventana de uso es limitada. El refresh token permite renovarlos de manera transparente para el usuario.

## Diferencias entre tokens

| Token | Duración | Uso |
|---|---|---|
| **Access Token** | Corta (minutos/horas) | Acceder a recursos |
| **Refresh Token** | Larga (días/semanas) | Obtener nuevos access tokens |

## Relación con otros conceptos

- [[OAuth 2.0]]: el flujo completo del que forma parte.
- [[OAuth 2.0 - Scope]]: los scopes del nuevo token son iguales o menores a los del token original.

## Referencia

https://goteleport.com/blog/how-oauth-authentication-works/
