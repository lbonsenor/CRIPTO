**HOTP** (_HMAC-Based One-Time Password_) es una variante de [[OTP - One Time Password]] basada en un contador y [[HMAC]]. Definida en el **RFC 4226**.

## Algoritmo

```
HOTP(K, C) = Truncate(HMAC-SHA1(K, C))
```

### Paso a paso de Truncate

Extrae **32 bits** de los 160 bits del MAC:

```
Offset = mac[19] & 0xf          ← 4 bits del último byte
bin    = mac[offset..offset+3] & 0x7fffffff
Result = bin % 10ⁿ              ← n = cantidad de dígitos deseados
```

## Parámetros

| Parámetro | Descripción |
|---|---|
| `K` | Clave compartida entre cliente y servidor |
| `C` | Contador (se incrementa en cada uso) |
| `n` | Cantidad de dígitos del código (1–9; se recomienda ≥ 6) |

## Propiedades y consideraciones

- El **contador se incrementa** en cada uso (cliente y servidor deben estar sincronizados).
- Requiere un mecanismo de **sincronización (_look-ahead_)**: el servidor acepta un rango de valores de contador para tolerar usos no registrados.
- Se aconseja usar **throttling**: limitar la cantidad de intentos para evitar ataques online.

## Relación con otros conceptos

- [[OTP - One Time Password]]: el concepto general del que es variante.
- [[TOTP]]: variante basada en timestamp en lugar de contador.
- [[HMAC]]: la primitiva criptográfica que usa internamente.
- [[Factor - Algo que tengo]]: el dispositivo físico que implementa HOTP.
