> [!example] Enunciado
> Una empresa desarrolla un chip subdérmico con **32 compartimentos de 1024 bytes**, cada uno con clave de escritura y clave de lectura (aleatorias, distintas entre sí, iguales en todos los chips). Comprar un espacio da el par de claves para ese compartimento.
>
> Escritura por NFC: `Header (4B "MTRX") || Size (2B) || Data (variable)`, todo cifrado con **AES-GCM** con la clave de escritura. Lectura: `Header (4B "RCAL") || Offset (2B) || Size (2B)`, cifrado con AES-GCM con la clave de lectura. Ambas tramas viajan por el mismo canal; el chip debe determinar qué tipo de operación es.
>
> a) Indicar paso a paso qué debe hacer el chip para validar y ejecutar un pedido de escritura.
> b) Indicar paso a paso qué debe hacer el chip para validar y ejecutar un pedido de lectura.
> c) Si no fuese posible usar AES-GCM, ¿qué características debe tener el criptosistema que lo reemplace?

**Conceptos:**

* [[GCM - Galois Counter Mode]]
* [[Message Authentication Code]]

a) **Paso 1:** el chip prueba descifrar la trama con cada una de las 64 claves (32 de escritura + 32 de lectura); AES-GCM incluye un tag de autenticación, así que solo la clave correcta hace que la verificación pase — el tipo de clave que funcionó determina si es escritura o lectura, y de qué compartimento. **Paso 2:** verifica que los primeros 4 bytes del texto plano sean "MTRX" (consistencia con clave de escritura). **Paso 3:** valida que `Size` esté entre 0 y 1024 y coincida con el largo real de `Data`. **Paso 4:** almacena `Data` en el compartimento correspondiente.

b) El paso 1 es el mismo (la clave que funcionó ya identificó una clave de lectura del compartimento `i`). **Paso 2:** verificar que el header sea "RCAL". **Paso 3:** validar que `Offset` esté entre 0 y 1023, `Size` entre 1 y 1024, y que `Offset + Size ≤ 1024` (evitando un buffer over-read que exponga datos de otros compartimentos). **Paso 4:** leer `Size` bytes desde `Offset`, cifrar la respuesta con AES-GCM usando la clave de lectura del compartimento y enviarla por NFC.

c) El reemplazo debe seguir siendo un esquema de **cifrado autenticado**: (1) cifrado, para que nadie con un lector NFC cercano pueda leer información médica o de contacto; (2) autenticación/integridad, la propiedad más crítica, ya que el chip usa el tag para determinar qué clave se usó (probando las 64 y viendo cuál valida el tag), garantizar integridad, y diferenciar lectura de escritura según qué tipo de clave valida. No alcanza con solo cifrado (no se sabría qué clave es correcta) ni con solo [[Message Authentication Code|MAC]] (se perdería confidencialidad). Ejemplos válidos: AES-CCM o Encrypt-then-MAC (AES-CBC + HMAC con claves independientes).
