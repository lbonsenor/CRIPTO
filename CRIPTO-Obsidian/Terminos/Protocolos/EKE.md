**EKE** (_Encrypted Key Exchange_) es una extensión del protocolo [[Challenge-Response]] que cifra el diálogo de autenticación para impedir que un atacante pueda **adivinar la clave** a partir del tráfico capturado.

## Problema que resuelve

En [[Challenge-Response]] básico, un atacante puede capturar el challenge `r` y la respuesta `f(k, r)` y realizar un ataque offline para adivinar `k`. EKE cierra esa ventana.

## Mecanismo

Todo el diálogo se realiza **dentro de un canal cifrado** con una clave de sesión `kₛ`:

```
A                                        B
|── {pedido de autenticación} ──────────►|
|◄──────────── { r }  ₖₛ  ──────────────|   ← Challenge cifrado
|──────────── f(k, r)  ₖₛ  ─────────────►|   ← Response cifrado
```

- `kₛ`: clave de sesión negociada previamente (ej: con [[Intercambio Diffie-Hellman]]).
- Un atacante que intercepta el tráfico ve datos cifrados; **no puede saber si una clave candidata es correcta** sin descifrar primero el canal.

## Propiedad clave

> El atacante no tiene forma de verificar si una clave adivinada es correcta, porque no puede distinguir un descifrado correcto de uno incorrecto.

## Relación con otros conceptos

- [[Challenge-Response]]: el protocolo base que extiende.
- [[Intercambio de Claves]]: mecanismo para establecer `kₛ`.
- [[Intercambio Diffie-Hellman]]: implementación típica del intercambio de clave de sesión.
- [[Criptosistema Asimétrico]]: puede usarse para cifrar el canal.
