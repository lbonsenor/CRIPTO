Patrón de ejercicio: "Diseñar autenticación segura contra atacantes X e Y" (ej: admin de base de datos, admin de red, atacante externo).

## Paso 1 — Identificar los atacantes y qué ve cada uno

| Atacante         | Qué ve                                 | Qué NO debe poder hacer                               |
| ---------------- | -------------------------------------- | ----------------------------------------------------- |
| Admin de BD      | Todo lo almacenado en la base de datos | Recuperar la contraseña/PIN original                  |
| Admin de red     | Todo lo que viaja por el canal         | Hacer [[Replay Attack\|replay]], extraer credenciales |
| Atacante externo | Nada (solo puede probar login)         | [[Fuerza Bruta]] online                               |

## Paso 2 — Elegir protección según el atacante

| Contra quién        | Protección                                          | Mecanismo                                                                    |
| ------------------- | --------------------------------------------------- | ---------------------------------------------------------------------------- |
| Admin de BD         | No guardar el PIN/contraseña en claro               | Hash lento con salt: $\text{bcrypt(salt+PIN)}$ — ver [[Bcrypt]], [[Salting]] |
| Admin de red        | No enviar el PIN/contraseña ni su hash por el canal | [[Challenge-Response]] con [[HMAC]]                                          |
| Ambos               | Combinar las dos capas                              | Hash lento en BD + challenge-response en red                                 |
| Fuerza bruta online | Rate limiting / bloqueo                             | Bloquear tras N intentos fallidos                                            |

## Paso 3 — Reglas de diseño

**Para la base de datos:**
- Nunca guardar contraseña/PIN en texto plano.
- Usar siempre un hash lento ([[Bcrypt]], [[Scrypt]], [[PBKDF2]]) si el espacio de valores es chico.
- Agregar siempre un [[Salting|salt]] aleatorio único por usuario.
- El salt no es secreto — protege contra [[Rainbow Table|rainbow tables]], no contra fuerza bruta dirigida.
- La protección real contra fuerza bruta viene de la lentitud del hash.

**Para la red:**
- Nunca enviar el PIN/contraseña en claro.
- Nunca enviar el hash bcrypt por la red — se convierte, de hecho, en "la contraseña".
- Usar [[Challenge-Response]]: el secreto nunca viaja, solo una prueba de que se lo conoce.
- El [[Nonce|nonce]]/challenge debe ser de uso único, invalidado después de cada intento.

**Lo que sí puede viajar en claro:** username/número de serie, [[Salting|salt]], [[Nonce|nonce]], el [[HMAC]] resultante.

**Lo que nunca debe viajar en claro:** el PIN/contraseña, o el hash bcrypt/scrypt del PIN (si el atacante lo captura, puede usarlo directamente como clave para futuros HMACs).

## Paso 4 — Protocolo tipo (plantilla)

```
1. App → Servidor: "quiero autenticarme, soy usuario X"
2. Servidor → App: nonce aleatorio + salt del usuario
3. App calcula localmente:
     hash = bcrypt(salt || PIN)
     respuesta = HMAC_{hash}(nonce)
4. App → Servidor: usuario X, respuesta
5. Servidor:
     - Busca el hash almacenado en BD para usuario X
     - Calcula HMAC_{hash_almacenado}(nonce)
     - Compara con la respuesta recibida
     - Invalida el nonce
```

## Paso 5 — Justificar inmunidad contra cada atacante

Plantilla de redacción: *"El admin de [BD/red] ve [listar exactamente qué ve]. No puede [acción] porque [razón criptográfica]. Para lograrlo necesitaría [qué tendría que hacer], lo cual es inviable porque [bcrypt es lento / HMAC no es invertible / el nonce es de único uso]."*

## Tabla de decisión: ¿qué hash usar?

| Situación | Hash recomendado | Por qué |
|---|---|---|
| Contraseña/PIN corto almacenado en BD | [[Bcrypt]] / [[Scrypt]] / [[PBKDF2]] | Lento → fuerza bruta inviable |
| Contraseña larga almacenada en BD | SHA-256 con salt puede alcanzar | Espacio grande → fuerza bruta ya es inviable igual |
| Integridad de un mensaje | [[HMAC]]-SHA256 | Rápido, no necesita ser lento |
| Commitment scheme | SHA-256 | Resistencia a colisiones |
| K-anonimato | SHA-256 | Determinístico, distribución uniforme |

## Tabla de decisión: ¿qué mecanismo usar según el canal?

| Canal seguro (HTTPS)                  | Canal NO seguro (HTTP)                                      |
| ------------------------------------- | ----------------------------------------------------------- |
| Enviar `hash(PIN)` directamente       | [[Challenge-Response]] con [[HMAC]]                         |
| TLS protege en tránsito               | El [[Nonce]] protege contra replay                          |
| Solo proteger la BD con bcrypt + salt | Proteger BD con bcrypt + salt **y** la red con HMAC + nonce |

## Errores comunes en estos ejercicios

- Enviar el hash bcrypt por la red — si el admin de red lo captura, puede usarlo como clave de HMAC para futuros challenges. Solo debe viajar el HMAC resultante.
- Usar SHA-256 para PINs cortos — el espacio (10⁸ combinaciones) se revienta en segundos. Usar [[Bcrypt]]/[[Scrypt]].
- Olvidar invalidar el nonce — si se puede reusar, el admin de red hace replay del HMAC capturado.
- Decir que el salt es secreto — nunca lo es. Protege contra rainbow tables, no contra fuerza bruta directa.
- No explicar por qué el atacante no puede — siempre cerrar con "necesitaría X, lo cual es inviable porque Y".
- Confundir autenticación con autorización: autenticación = "¿quién sos?"; autorización = "¿qué podés hacer?" (ver [[Control de Acceso]]).
