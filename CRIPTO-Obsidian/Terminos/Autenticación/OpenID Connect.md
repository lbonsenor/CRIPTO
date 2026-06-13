**OpenID Connect (OIDC)** es el protocolo de [[Autenticación]] moderno y estándar para [[SSO - Single Sign-On]]. Está construido **sobre [[OAuth 2.0]]**, al que agrega una capa de identidad.

## Diferencia con OAuth 2.0

| Aspecto | [[OAuth 2.0]] | OpenID Connect |
|---|---|---|
| Propósito | **Autorización** (acceso a recursos) | **Autenticación** (verificación de identidad) |
| Token principal | _Access Token_ | _ID Token_ (JWT) |
| Información de usuario | No incluida por defecto | Incluida en el ID Token |

## Componentes principales

- **ID Token**: JWT (_JSON Web Token_) firmado que contiene información de identidad del usuario (claims: nombre, email, etc.).
- **UserInfo Endpoint**: endpoint donde se puede obtener información adicional del usuario usando el Access Token.
- **Discovery**: endpoint estándar (`/.well-known/openid-configuration`) que expone la configuración del IdP.

## Flujo básico

```
Usuario ──── Login ────► Identity Provider (IdP)
                                 │
                          ID Token + Access Token
                                 │
Aplicación ──── Verifica ID Token ────► Acceso concedido
```

## Versiones anteriores

- **OpenID 1.0 / 2.0**: obsoletos. Fueron reemplazados por OpenID Connect debido a su mayor complejidad de implementación y limitaciones de seguridad.

## Relación con otros conceptos

- [[OAuth 2.0]]: el estándar base sobre el que se construye.
- [[OAuth 2.0 - Scope]]: OIDC agrega scopes estándar como `openid`, `profile`, `email`.
- [[SSO - Single Sign-On]]: el caso de uso principal de OIDC.
- [[Firma Digital]]: el ID Token es un JWT firmado digitalmente.
- [[Autenticación]]: el proceso que OIDC implementa.
