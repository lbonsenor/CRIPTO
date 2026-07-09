GCM combina cifrado en modo [[Counter (CTR)]] con una función de autenticación basada en aritmética en GF(2^128), produciendo en un solo paso confidencialidad **y** integridad. Es la respuesta típica cuando un ejercicio dice que un esquema "no da integridad": en vez de usar [[CBC]] o un cifrado de flujo puro y agregar un [[Message Authentication Code]] por separado (con el riesgo de usar la misma clave para ambos, lo cual rompe [[Chosen Ciphertext Attack|CCA-security]]), AES-GCM deriva y separa internamente el proceso de cifrado del de autenticación.

Requisitos de seguridad:
- El nonce (IV) debe ser único por mensaje bajo la misma clave. Reutilizar un nonce con GCM rompe completamente la autenticación (permite forjar tags).
- No requiere un [[Message Authentication Code]] adicional: el tag de GCM ya cumple ese rol.
- Sigue siendo simétrico: ambas partes comparten la clave, a diferencia de una [[Firma Digital]], por lo que no ofrece no-repudio.

Se usa como reemplazo directo de [[CBC]], [[CFB]], [[OFB]] cuando el enunciado pide cifrado autenticado.
