Ataque que prueba sistemáticamente todas las claves, contraseñas o PINs posibles hasta encontrar el correcto. Su viabilidad se estima como:

`Tiempo total = (tamaño del espacio de claves) × (tiempo por intento)`

Ejemplos de espacio de búsqueda:
- PIN de 4 dígitos: 10⁴ = 10.000 combinaciones
- PIN de 8 dígitos: 10⁸ = 100.000.000 combinaciones
- Contraseña de 8 caracteres alfanuméricos: ~62⁸ ≈ 2×10¹⁴ combinaciones

Con un hash rápido como SHA-256 (~1 μs por intento), un PIN de 8 dígitos se rompe en ~100 segundos: inseguro. Con un hash lento como [[Bcrypt]] (~100 ms por intento), el mismo PIN tarda del orden de 115 días: seguro. Este es el motivo por el que la defensa contra fuerza bruta offline no viene de "esconder" el hash sino de que la función de hash sea deliberadamente lenta ([[Bcrypt]], [[Scrypt]], [[PBKDF2]]).

Contra fuerza bruta **online** (probando contra el servidor en vivo) la defensa es distinta: rate limiting / bloqueo de cuenta tras N intentos fallidos.
