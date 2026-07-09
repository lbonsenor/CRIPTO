> [!example] Enunciado
> Considerar una BD de emails usados para SPAM y un servicio que permite a cualquiera consultar si su email está en la lista, manteniendo el anonimato mediante **k-anonymity**: el servicio guarda `SHA-256(email)`, y un cliente envía solo los primeros `x` bits del hash de su email, recibiendo todos los hashes de la BD con ese prefijo.
>
> 1. ¿Mejora la seguridad usar SALT de 256 bits para almacenar los hashes? ¿Tiene otra consecuencia?
> 2. Explicar por qué alguien que controla el servidor no puede saber ni el email consultado ni si la búsqueda fue exitosa.
> 3. ¿Con qué criterio se define `x` (bits de prefijo) para garantizar 256-anonymity?

**Conceptos:**

* [[K-Anonimato]]
* [[Salting]]
* [[Funciones de Hash criptograficas]]

1. **No**, no mejora la seguridad y de hecho **rompe la funcionalidad completa del esquema**. El salt hace que el hash sea no determinístico: para un mismo email, `SHA-256(salt || email)` da un resultado distinto según el salt. Esto genera dos problemas: el cliente no puede consultar (necesitaría conocer el salt exacto que usó el servidor para ese email, que no tiene), y aunque el servidor devolviera hashes con el prefijo pedido, el hash completo calculado por el cliente nunca coincidiría con el almacenado. El [[Salting|salt]] es útil cuando el **servidor** hace la comparación (contraseñas), pero acá quien compara es el **cliente**.

2. Dos razones: **(a)** resistencia a preimagen de SHA-256 — el servidor recibe solo un prefijo y no puede invertirlo para hallar el email original; **(b)** [[K-Anonimato|k-anonymity]] por prefijo — los primeros `x` bits coinciden con al menos `k` emails de la BD, y el servidor no tiene forma de saber cuál de ellos busca el cliente. Además, la comparación final (verificar si el hash completo está entre los resultados) la hace el **cliente localmente**; el servidor nunca recibe el hash completo y no puede saber si la búsqueda fue exitosa.

3. Con `x` bits de prefijo, los hashes se dividen en `2^x` grupos de aproximadamente `|BD| / 2^x` elementos cada uno (asumiendo distribución uniforme). Para garantizar `k`-anonymity se necesita `|BD| / 2^x ≥ k`, es decir `x ≤ log₂(|BD| / k)`. Para 256-anonymity: `x ≤ log₂(|BD| / 256)`. Es un trade-off entre anonimato (x chico) y eficiencia de la respuesta (x grande).
