**TOTP** (_Time-Based One-Time Password_) es una variante de [[OTP - One Time Password]] que reemplaza el contador de [[HOTP]] por un **timestamp**. Definida en el **RFC 6238**.

## Algoritmo

```
TOTP(K) = HOTP(K, T)
```

Donde `T` se calcula a partir del tiempo actual:

```
T = ⌊(current_unix_time - T₀) / X⌋
```

| Variable | Descripción | Valor típico |
|---|---|---|
| `T₀` | Tiempo de referencia (época) | `0` (Unix epoch) |
| `X` | Duración de cada código en segundos | `30` segundos |

## Propiedades y consideraciones

- **No requiere un contador** compartido → más simple de implementar y sin problemas de desincronización de contadores.
- Se aconseja usar un **drift-window**: aceptar códigos generados en un rango de ticks desfasados (ej: ±1 tick = ±30s) para tolerar diferencias de reloj.
- Se aconseja usar **throttling**: limitar la cantidad de intentos.
- Genera códigos de 1 a 9 dígitos; se recomienda **al menos 6**.
- Es la base de la mayoría de las apps de autenticación 2FA (Google Authenticator, Authy, etc.).

## Comparación con HOTP

| Característica | [[HOTP]] | TOTP |
|---|---|---|
| Base | Contador `C` | Timestamp `T` |
| Sincronización | Contador (look-ahead) | Reloj (drift-window) |
| Validez del código | Hasta el próximo uso | ~30 segundos |
| Riesgo de phishing | Mayor (código válido indefinidamente) | Menor (caduca rápido) |

## Relación con otros conceptos

- [[OTP - One Time Password]]: el concepto general del que es variante.
- [[HOTP]]: la variante con contador en la que se basa.
- [[HMAC]]: la primitiva criptográfica subyacente.
- [[Factor - Algo que tengo]]: el dispositivo o app que genera el código.
- [[Multi-Factor Authentication]]: TOTP es el segundo factor más común.
