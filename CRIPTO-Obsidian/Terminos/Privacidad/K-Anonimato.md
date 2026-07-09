Propiedad de un esquema donde una consulta revela información suficiente para identificar un grupo de al menos `k` individuos indistinguibles entre sí, en vez de un único individuo. Se estima con la fórmula:

`k ≈ |BD| / 2^x`

donde `x` es la cantidad de bits del prefijo (por ejemplo, de un hash) que se envían al servidor, y `|BD|` es el tamaño total de la base de datos. Para garantizar un `k` mínimo deseado: `x ≤ log₂(|BD| / k)`.

Un caso típico: un cliente calcula el hash de un dato sensible (por ejemplo un email) usando una [[Funciones de Hash criptograficas|función de hash]] y envía solo los primeros `x` bits al servidor. El servidor responde con todos los registros cuyo hash comparte ese prefijo (k resultados), y es el **cliente** quien compara localmente cuál corresponde al dato real — el servidor nunca sabe cuál de los k matcheó, ni si la consulta fue "exitosa".

Detalle importante: agregar [[Salting|salt]] a este esquema **rompe la funcionalidad**, porque el cliente ya no podría recalcular el mismo hash que el servidor usó para indexar los registros.
