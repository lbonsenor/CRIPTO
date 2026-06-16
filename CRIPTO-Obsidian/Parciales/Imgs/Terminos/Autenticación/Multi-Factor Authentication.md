**MFA** (_Multi-Factor Authentication_) consiste en requerir **dos o más factores de diferente tipo** para autenticar a una entidad.

## Racional

Cada tipo de [[Factor de Autenticación]] requiere comprometer distintos _assets_:

- Robar una **contraseña** no alcanza si también se necesita el **celular**.
- Clonar un **token físico** no alcanza si también se necesita la **huella digital**.

## Combinaciones comunes

| Combinación | Factor 1 | Factor 2 |
|---|---|---|
| Más frecuente | [[Factor - Algo que conozco]] (clave) | [[Factor - Algo que tengo]] (OTP/celular) |
| Alta seguridad | [[Factor - Algo que conozco]] (clave) | [[Factor - Algo que soy]] (biometría) |
| Sin clave | [[Factor - Algo que tengo]] (token) | [[Factor - Algo que soy]] (biometría) |

## Implementaciones comunes del segundo factor

- **SMS / Email OTP**: código enviado al celular o correo (vulnerable a SIM swapping).
- **App de autenticación**: [[TOTP]] en Google Authenticator, Authy, etc.
- **Hardware token**: [[HOTP]]/[[TOTP]] en dispositivo físico (ej: YubiKey).
- **Push notification**: aprobación desde una app móvil.
- **Biometría**: huella, cara, iris en el dispositivo.

## Relación con otros conceptos

- [[Factor de Autenticación]]: los tipos de factores que combina.
- [[TOTP]] y [[HOTP]]: implementaciones típicas del segundo factor.
- [[Factor - Contexto]]: a veces se usa como factor adicional silencioso.
- [[SSO - Single Sign-On]]: MFA suele ser requerido en sistemas SSO.
