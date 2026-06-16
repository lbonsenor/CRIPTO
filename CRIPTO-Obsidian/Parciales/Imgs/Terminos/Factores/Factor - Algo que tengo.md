El factor **"algo que tengo"** basa la identificación en la **posesión de un elemento físico**.

## Ejemplos

- Smart card / USB token
- Generador de claves (ej: RSA SecurID)
- Celular (ej: recibir un SMS o usar una app de autenticación)
- Tarjeta de coordenadas (_Token Grid_)

## Características

- Requiere que la entidad tenga el **elemento físico en su posesión** al momento de autenticarse.
- Introduce una barrera adicional: el atacante debe comprometer algo físico, no solo un secreto.

## Debilidades

- El elemento puede ser **copiado o clonado**.
- La información en tránsito puede ser **suplantada** (ej: intercepción de SMS).
- Si se pierde o roba el elemento, se pierde la capacidad de autenticación.

## Implementaciones basadas en OTP

Los generadores de claves físicos (tokens) suelen implementar:

- [[HOTP]]: OTP basado en contador HMAC.
- [[TOTP]]: OTP basado en timestamp.

## Relación con otros conceptos

- [[Factor de Autenticación]]: categoría a la que pertenece.
- [[OTP - One Time Password]]: mecanismo típico de este factor.
- [[HOTP]]: variante basada en contador.
- [[TOTP]]: variante basada en tiempo.
- [[Multi-Factor Authentication]]: se combina con [[Factor - Algo que conozco]] u otros.
