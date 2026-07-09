Tabla para mapear rápidamente una pista del enunciado a la primitiva que probablemente se está buscando.

| Pista del enunciado | Primitiva | Justificación |
|---|---|---|
| "No da integridad" | [[GCM - Galois Counter Mode\|AES-GCM]] (reemplaza [[CBC]]/[[Counter (CTR)\|CTR]]/[[ChaCha20]]) | Cifrado autenticado con una sola clave |
| "No puede usar nuevas claves fácilmente" | [[GCM - Galois Counter Mode\|AES-GCM]] | Deriva claves internamente |
| "Puede falsificar" | [[Firma Digital]] ([[Hashed RSA]], [[Digital Signature Standard]]) | No repudio, solo la clave privada genera la firma |
| "Replay attack" | Counter o timestamp ([[Denning-Sacco]]) | Counter si no hay reloj, timestamp si hay reloj |
| "Alguien debe poder verificar sin poder falsificar" | [[Firma Digital]] (pk verifica, sk firma) | Menor privilegio |
| "Dos partes verifican entre sí" | [[Message Authentication Code|MAC]] ([[HMAC]]) | Eficiente, clave compartida |
| "Terceros deben poder verificar" | [[Firma Digital]] | La clave pública es, justamente, pública |
| "Campo no protegido por el MAC" | Incluir el campo dentro del MAC | Integridad de todos los datos |
| "Token robado vale para siempre" | Timestamp + ventana de validez | Análogo a [[Denning-Sacco]] |
| "Token válido en cualquier servicio" | Agregar campo de servicio/destino al token | Vincula el token a su destino |
| "Cifrar y hacer MAC con la misma clave" | No es [[Chosen Ciphertext Attack\|CCA-secure]] | Usar claves independientes o cifrado autenticado |
| "Espacio de mensajes chico" | Agregar [[Nonce\|nonce]] aleatorio | `h(m \|\| r)` en vez de `h(m)` |
