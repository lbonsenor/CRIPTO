Ejercicios sobre esquemas de privacidad donde el cliente consulta al servidor sin revelarle exactamente qué dato busca.

## Método de resolución

1. Entender la fórmula de [[K-Anonimato]]: `k ≈ |BD| / 2^x`, donde `x` son los bits de prefijo enviados y `k` el mínimo de resultados que "esconden" al usuario dentro del grupo.
2. Para garantizar un `k` mínimo deseado: `x ≤ log₂(|BD| / k)`.
3. Preguntas típicas del final:
   - ¿Agregar [[Salting|salt]] mejora el esquema? **No** — rompe la funcionalidad, porque el cliente ya no puede recalcular el mismo hash que usa el servidor para indexar.
   - ¿Por qué el servidor no puede saber el dato exacto? Por resistencia a preimagen del hash usado más el hecho de que el prefijo enviado matchea `k` registros, no uno solo.
   - ¿Por qué el servidor no sabe si la consulta fue exitosa? Porque la comparación final la hace el **cliente**, localmente, sobre los `k` resultados recibidos.
