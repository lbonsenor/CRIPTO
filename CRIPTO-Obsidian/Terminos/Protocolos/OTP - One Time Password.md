> ⚠️ No confundir con [[Secreto Perfecto|One Time Pad]].

Un **OTP** (_One Time Password_) es una clave que sirve **una única vez**. Implementa el [[Factor - Algo que tengo]] cuando se genera en un dispositivo físico.

## Concepto

A y B comparten una clave `K` y un contador `C`:

```
(K, C)  →  OTP  →  (otp, C')
```

Cada uso consume el contador (o el tiempo), generando un código diferente.

## Variantes estandarizadas

| Variante | Base | RFC | Archivo |
|---|---|---|---|
| **HOTP** | HMAC + contador | RFC 4226 | [[HOTP]] |
| **TOTP** | HMAC + timestamp | RFC 6238 | [[TOTP]] |

## Propiedades

- El código es válido **una sola vez** (o por un período muy corto en TOTP).
- Resiste ataques de **replay**: interceptar un OTP ya usado no sirve.
- No requiere transmitir la clave compartida `K`.

## Relación con otros conceptos

- [[HOTP]]: variante basada en contador HMAC.
- [[TOTP]]: variante basada en timestamp.
- [[Factor - Algo que tengo]]: el dispositivo generador implementa este factor.
- [[HMAC]]: primitiva criptográfica base de ambas variantes.
- [[Challenge-Response]]: protocolo alternativo para autenticación sin transmitir claves.
- [[Multi-Factor Authentication]]: los OTPs suelen ser el segundo factor.
