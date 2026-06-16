El **Challenge-Response** es un protocolo de [[Autenticación]] que permite verificar la identidad de un usuario remoto **sin transmitir la clave** por el canal.

## Problema que resuelve

La autenticación con claves simple requiere enviar la clave al servidor, lo que:
- Requiere un canal seguro.
- Expone la clave si se intercepta una comunicación pasada.

## Protocolo

```
A                              B
|── {pedido de autenticación} ──►|
|◄────────── { r }  ─────────────|   ← Challenge (número aleatorio)
|──────── f(k, r)  ──────────────►|   ← Response (función de la clave y el challenge)
```

- `k`: clave compartida entre A y B.
- `r`: challenge aleatorio generado por B (cambia en cada sesión).
- `f(k, r)`: función que demuestra conocimiento de `k` sin revelarla (generalmente un [[Message Authentication Code]]).

## Propiedades

- La clave **nunca se transmite**.
- El challenge `r` es diferente en cada sesión → resistente a **ataques de replay**.
- Un atacante que intercepta `f(k, r)` no puede reutilizarlo en una sesión futura (el challenge será diferente).

## Limitación

Un atacante en la red puede capturar `r` y `f(k, r)` y realizar un **ataque offline** para adivinar `k`. Solución: ver [[EKE]].

## Relación con otros conceptos

- [[EKE]]: extensión que cifra el diálogo para evitar ataques offline.
- [[OTP - One Time Password]]: alternativa que también evita la transmisión de la clave.
- [[Factor - Algo que conozco]]: el factor que implementa.
- [[Message Authentication Code]]: la primitiva criptográfica típicamente usada en `f`.
