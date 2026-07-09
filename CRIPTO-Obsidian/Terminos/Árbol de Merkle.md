Estructura que permite verificar la integridad de partes de un mensaje grande sin tener que recalcular el hash de todo el mensaje. Cada hoja del árbol contiene el hash de un bloque del mensaje, y cada nodo interno contiene el hash de la concatenación de sus dos hijos, hasta llegar a una raíz única (Top Hash / Root Hash).

Propiedades relevantes (asumiendo una [[Funciones de Hash criptograficas|función de hash]] libre de colisiones):
- Para verificar un bloque individual alcanza con recalcular su hash de hoja y subir por el camino hasta la raíz usando los hashes "hermanos" de ese camino — O(log n) hashes en vez de recalcular todo el mensaje.
- Verificar solo los hashes intermedios de una rama (sin llegar a la raíz) **no** es suficiente: un atacante puede modificar bloques y recalcular consistentemente los hashes intermedios falsos. Solo la raíz (compartida por un canal de confianza) ancla la integridad de todo el árbol.
- Un Merkle Tree "ingenuo" puede no ser resistente a segunda preimagen: si no se distingue entre nodos hoja y nodos internos, un atacante puede presentar los hashes intermedios como si fueran datos crudos de un mensaje más chico y obtener la misma raíz. Se soluciona agregando un prefijo distinto al hashear hojas (`h(0x00 || datos)`) y nodos internos (`h(0x01 || hijo_izq || hijo_der)`).
- Si alguien encuentra una segunda preimagen de un bloque hoja (rompiendo la función de hash subyacente), puede propagar esa segunda preimagen hasta la raíz — pero esto no es una debilidad del árbol, sino de la función de hash usada.

Uso típico: verificar transferencias de archivos grandes por bloques, permitiendo re-enviar solo el bloque con error y el camino de hashes hasta la raíz (O(log n) datos) en vez de todo el mensaje.
