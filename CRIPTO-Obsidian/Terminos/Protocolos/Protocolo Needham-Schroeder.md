Protocolo de intercambio de claves **simétricas** que utiliza un [[Key Distribution Centers|KDC]] (Key Distribution Center) centralizado. Es la base del protocolo [[Kerberos]] y de Active Directory (con modificaciones).

## Supuestos

- Cada entidad comparte una clave secreta con el KDC
- El KDC es de confianza para todas las partes

## Primera aproximación (ingenua)

```
A → KDC: "Sesión A → B"
KDC → A: { ks }ka || { ks }kb
A → B: { ks }kb
```

### Problemas

- **Replay attack**: un atacante puede reenviar `{ ks }kb` y hacerse pasar por A ante B
- **Key reuse**: un atacante puede reenviar el mensaje del KDC a A para forzar la reutilización de una clave de sesión antigua

## Segunda aproximación (con nonces)

```
A → KDC: A || B || r1
KDC → A: { A || B || r1 || ks || { A || ks }kb }ka
A → B:   { A || ks }kb
B → A:   { r2 }ks
A → B:   { r2 - 1 }ks
```

Los [[OTP - One Time Password|nonces]] `r1` y `r2` previenen replay attacks.

### Problema residual (ataque de Lowe)

Si un atacante E obtiene una **clave de sesión antigua** `ks`, puede ejecutar el protocolo desde el paso 3:

```
E → B: { A || ks }kb
B → E: { r2 }ks
E → B: { r2 - 1 }ks
```

E logra **impersonar a A** ante B.

## Modificación Denning-Sacco (con timestamps)

Solución: agregar un **timestamp T** dentro del ticket cifrado con `kb`:

```
A → KDC: A || B || r1
KDC → A: { A || B || r1 || ks || { A || T || ks }kb }ka
A → B:   { A || T || ks }kb
B → A:   { r2 }ks
A → B:   { r2 - 1 }ks
```

El timestamp permite a B detectar que el ticket es antiguo y rechazarlo. Requiere **sincronización de relojes** entre las partes.

## Relación con Kerberos

[[Kerberos]] implementa una versión modificada de Needham-Schroeder con:
- Timestamps para prevenir replay
- Tickets de servicio adicionales
- Ticket Granting Ticket (TGT)

## Ver también

- [[Key Distribution Centers]]
- [[Intercambio de Claves]]
- [[Kerberos]]
- [[OTP - One Time Password]]
- [[Ataques a Sistemas de Autenticación]]
