![[2025-Q1-R-EJ4.png]]
**Conceptos**
- [[Data Encryption Standard (DES)]]
- [[3-DES]]
- [[AES]]
- [[Funciones de Hash criptograficas]]
- [[OTP]]
- [[Principios de Kerchoffs]]
### a) Para cualquier sistema que requiera mantener la confidencialidad de un canal, el algoritmo de DES puede usarse sin inconvenientes.
**FALSO**
Para cualquier sistema que requiera mantener la confidencialidad de un canal, el algoritmo de **AES (o Triple DES / 3DES)** puede usarse sin inconvenientes.

### b) Una función de hash es una buena alternativa para encriptar un mensaje.
**FALSO**
Una función de hash **no es una alternativa para encriptar un mensaje, sino para garantizar su integridad (o autenticidad)**. _(Alternativa de redacción directa: Un algoritmo de cifrado simétrico o asimétrico es una buena alternativa para encriptar un mensaje)._

### c) One-Time-Pad es un algoritmo seguro contra ataques de texto plano exclusivamente.
**FALSO**
One-Time-Pad es un algoritmo seguro **frente a cualquier adversario con capacidad de cómputo ilimitada (posee seguridad perfecta / incondicional)**.

### d) El principio de Kerkhoff establece que la seguridad de un criptosistema sólo depende de que el algoritmo de encripción esté bien protegido.
**FALSO**
El principio de Kerckhoffs establece que la seguridad de un criptosistema **debe residir únicamente en mantener el secreto de la clave, y no en mantener en secreto el algoritmo de encripción (el cual se asume público)**.