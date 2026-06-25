Dado $H(x)$ una función de hash libre de colisiones

$HMACk(x)$
- $\text{Gen}:k\leftarrow K,s\leftarrow S$
- $\text{Mac}: t=H^s((k\oplus \text{opad})||H^{s}((k\oplus\text{ipad})||m))$
- $\text{opad}=0x36\dots36$
- $\text{ipad}:0x 5c 5c\dots 5c$

- **Inmune a ataques de extensión de longitud:** A diferencia de las funciones hash por sí solas, el diseño de doble capa de HMAC elimina por completo la vulnerabilidad a ataques de extensión de longitud.
    
- **Seguro con longitud variable:** Maneja de forma nativa y segura mensajes de cualquier tamaño, sin necesidad de parches o variantes complejas como en CBC-MAC.
    
- **Velocidad y rendimiento:** Las funciones hash (como SHA-256 o BLAKE2) suelen ser extremadamente rápidas cuando se implementan en software, superando a los cifrados por bloques en muchas arquitecturas.
    
- **Flexibilidad:** Es independiente de la función hash subyacente. Si SHA-256 se vuelve inseguro en el futuro, puedes migrar a SHA-3 o BLAKE3 cambiando solo el algoritmo base, manteniendo la estructura de HMAC.
    

### Desventajas:

- **Mayor consumo de recursos (en ciertos entornos):** Si un dispositivo de muy bajos recursos (como una tarjeta inteligente) ya tiene un chip físico para AES, implementar HMAC por software requiere memoria y código adicional para la función hash.
    
- **Tamaño de la clave:** Requiere un manejo cuidadoso del tamaño de la clave. Si la clave es más larga que el tamaño del bloque del hash, primero debe pasarse por el hash, lo que añade un pequeño paso extra.