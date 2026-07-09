> [!example] Enunciado
> Se quiere enviar un conjunto de datos muy grande, particionado en bloques, evitando reenviar todo el mensaje si un bloque falla. Alternativas al MAC tradicional:
>
> 1. `MAC'_i = bloque_anterior || MAC(bloque_i)` (con 0s en el bloque 0).
> 2. `MAC` de la concatenación de los `MAC` de cada bloque.
> 3. `MAC` usando un árbol de Merkle.
>
> Evaluar para cada caso si tiene integridad y, si la tiene, cómo se manejaría un bloque con dato erróneo sin reenviar todo el mensaje.

**Conceptos:**

* [[Message Authentication Code]]
* [[Árbol de Merkle]]

**Opción 1: MAC encadenado — ❌ NO tiene integridad.** Es vulnerable a truncamiento: un atacante puede eliminar bloques del final y el receptor no lo detecta, porque cada `MAC'` solo depende del bloque anterior, no de los siguientes.

**Opción 2: MAC de MACs — ✅ SÍ tiene integridad.** `MAC_final = MAC(MAC(bloque_1) || ... || MAC(bloque_n))` depende de todos los bloques; modificar, agregar o quitar cualquiera cambia el MAC final. **Problema:** si un bloque tiene error, no se identifica directamente cuál — hay que recalcular todos los MAC individuales (O(n)), y hay que reenviar todo el mensaje porque el MAC final cambia al corregir cualquier bloque.

**Opción 3: [[Árbol de Merkle]] — ✅ SÍ tiene integridad, y localiza el error en O(log n).** La raíz depende de todos los bloques. Para encontrar el bloque con error se recorre el árbol desde la raíz: se comparan los dos hijos para ver en qué mitad está el error, se baja por esa rama, y se repite hasta llegar al bloque exacto en O(log n) comparaciones. Solo hace falta reenviar el bloque erróneo y los hashes del camino hasta la raíz (O(log n) datos), en vez de todo el mensaje.
