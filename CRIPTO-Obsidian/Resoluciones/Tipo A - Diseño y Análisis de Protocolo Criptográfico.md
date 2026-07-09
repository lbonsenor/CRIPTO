Ejercicios típicos: SSO, códigos QR, cámaras de seguridad, sensores en un oleoducto, transporte de valores, elección de un supervisor.

## Método de resolución

**1. Leer el protocolo completo y anotar:**
- ¿Qué datos se envían?
- ¿Qué está cifrado y qué va en texto plano?
- ¿Qué algoritmo se usa? ([[CBC]], [[GCM - Galois Counter Mode]], [[ChaCha20]], [[Firma Digital]], [[Message Authentication Code]], función de hash)
- ¿Quién tiene qué claves?

**2. Identificar qué propiedades se necesitan:**
- Confidencialidad → cifrado
- Integridad → [[Message Authentication Code|MAC]] o cifrado autenticado
- No repudio → [[Firma Digital]]
- Autenticación → firma, MAC, [[Challenge-Response]]
- Anti-replay → counter, timestamp, [[Nonce]]

**3. Buscar lo que falta:**
- ¿El MAC/firma cubre TODOS los campos del mensaje? Si no, hay [[Bit-Flipping Attack|bit-flipping]] o modificación de campos no protegidos.
- ¿Hay protección contra [[Replay Attack|replay]]? Si no, agregar counter o timestamp (ver [[Denning-Sacco]]).
- ¿El cifrado da integridad por sí solo? [[CBC]] y [[ChaCha20]] **no** dan integridad.
- ¿Las claves se comparten innecesariamente entre muchas partes? Aplicar menor privilegio ([[Least Privilege]]), preferir [[Firma Digital]] sobre MAC compartido.
- ¿Hay expiración? Si no, los tokens/tickets son válidos para siempre.

**4. Proponer solución y justificar:**
- Nombrar el principio teórico (CIA — ver [[La triada]] —, [[Principios de Diseño de Seguridad|Saltzer y Schroeder]], o una propiedad formal como [[Chosen Ciphertext Attack|CCA-security]]).
- Mostrar cómo queda la estructura del mensaje modificada.
- Explicar por qué el atacante ya no puede ejecutar el ataque original.

## Checklist de ataques comunes en protocolos

| Si el protocolo tiene… | Buscar… | Solución |
|---|---|---|
| [[CBC]] o cifrado de flujo sin MAC | [[Bit-Flipping Attack]] | [[GCM - Galois Counter Mode|AES-GCM]] o agregar MAC con clave independiente |
| Mensaje sin counter ni timestamp | [[Replay Attack]] | Agregar counter (sin reloj) o timestamp (con reloj, ver [[Denning-Sacco]]) |
| Clave simétrica compartida con muchas partes | Compromiso de clave permite falsificar | Cambiar a [[Firma Digital]] (menor privilegio) |
| MAC que no cubre todos los campos | Modificación de campos no protegidos | Incluir TODOS los campos dentro del MAC |
| Token sin expiración | Token robado vale para siempre | Agregar timestamp + ventana de validez ([[Denning-Sacco]]) |
| Token sin campo de destino | Token válido en cualquier servicio | Agregar campo servicio/destino al token |
| Sin autenticación de origen | Suplantación / masquerading | [[Firma Digital]] |
| Clave compartida entre dos partes con MAC | No hay no-repudio | Cambiar a [[Firma Digital]] |
