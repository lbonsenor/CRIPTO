Ataque donde un adversario captura un mensaje válido (credencial, token, respuesta de autenticación) y lo reenvía más tarde para hacerse pasar por el emisor original, sin necesidad de romper ninguna primitiva criptográfica.

Mitigaciones típicas:
- **Counter**: cada mensaje incluye un contador incremental; el receptor rechaza valores ya vistos o menores. Útil cuando no hay reloj sincronizado.
- **Timestamp**: cada mensaje incluye la hora de emisión y una ventana de validez (ver [[Denning-Sacco]]). Requiere relojes razonablemente sincronizados.
- **Nonce de único uso**: el receptor invalida el desafío después de cada intento (ver [[Nonce]] y [[Challenge-Response]]).

Un error común es decir "hay replay attack" sin explicar contra qué campo del mensaje aplica y qué efecto concreto tiene.
