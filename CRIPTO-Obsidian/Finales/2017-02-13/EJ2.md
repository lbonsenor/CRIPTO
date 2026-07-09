> [!example] Enunciado
> Un sistema de puntos de venta para una cadena de supermercados corre en un Linux modificado tipo Kiosk, con un servicio REST en la red interna (PostgreSQL + microservicios). Cada cajero pasa una tarjeta (número de serie) e ingresa un PIN de 8 dígitos. El servicio expone `POST /session` con `{ "user": "<número de serie>", "credentials": "<info de autenticación>" }`. En la intranet no se permite HTTPS (por balance de carga y auditoría), solo HTTP.
>
> Los dos ataques más peligrosos a mitigar son: el administrador de BD queriendo impersonar a un cajero, y el administrador de red (que captura mensajes) queriendo impersonar a un cajero.
>
> 1. Definir cómo implementaría el servicio de autenticación (qué se guarda en la BD, cómo se conforma "credentials", cómo se autentica al cajero).
> 2. Explicar por qué el sistema resultante es inmune a ambos escenarios de ataque.

**Conceptos:**

* [[Bcrypt]]
* [[Salting]]
* [[Challenge-Response]]
* [[HMAC]]
* [[Nonce]]
* [[Fuerza Bruta]]

**a) Qué se guarda en la BD:** `numero_de_serie | salt | bcrypt(salt || PIN)`. Se usa [[Bcrypt|bcrypt]] (o [[PBKDF2]]) por ser una función de hash lenta, necesaria porque el PIN de 8 dígitos tiene solo 10^8 combinaciones (con un hash rápido como SHA-256 la [[Fuerza Bruta|fuerza bruta]] sería trivial).

**b) Protocolo (challenge-response):** `App → Servidor: GET /challenge?user=#serie` → `Servidor → App: { nonce, salt }`. Luego `App → Servidor: POST /session { "user": "#serie", "credentials": HMAC_{bcrypt(salt || PIN)}(nonce) }`. La app calcula localmente `bcrypt(salt || PIN)` y lo usa como clave de un [[HMAC]] sobre el [[Nonce|nonce]] recibido — nunca envía el PIN ni el hash bcrypt.

**c) Autenticación:** el servidor busca el `bcrypt(salt || PIN)` almacenado, recupera el nonce que había emitido, calcula `HMAC_{bcrypt_almacenado}(nonce)`, y compara con lo recibido. Invalida el nonce en ambos casos (éxito o fallo).

**Por qué es inmune:**
- **Admin de BD** ve `numero_de_serie`, `salt` y `bcrypt(salt || PIN)`. No puede recuperar el PIN porque bcrypt no es invertible, y la fuerza bruta sobre 10^8 combinaciones a ~100ms por intento tardaría meses. Tampoco tiene acceso a la red para usar ese hash en un challenge en vivo.
- **Admin de red** ve solo `nonce`, `salt` (ambos públicos) y el `HMAC` resultante. No puede hacer replay porque el nonce es de un solo uso; no puede extraer el PIN ni el hash bcrypt del HMAC (no invertible); y no puede fabricar un HMAC válido para un nonce nuevo porque necesitaría `bcrypt(salt || PIN)` como clave, que nunca viaja por la red.
