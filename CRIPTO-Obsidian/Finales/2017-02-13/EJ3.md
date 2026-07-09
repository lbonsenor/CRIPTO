> [!example] Enunciado
> Para verificar parcialmente la integridad de un conjunto de datos grande (por ejemplo, un mensaje de 1TB donde interesa verificar 1MB) se usa una construcción de **Árbol de Merkle**, donde cada hoja lleva el hash de un subconjunto del mensaje y cada nodo interno el hash de sus hijos. Suponiendo una función de hash libre de colisiones:
>
> 1. ¿Por qué para verificar la integridad del bloque 001 solo es necesario calcular H0-1?
> 2. ¿Por qué para verificar la segunda mitad del mensaje NO alcanza con verificar H1-0 y H1-1?
> 3. Un Merkle Tree sin más modificaciones puede no ser resistente a segundas preimágenes. ¿Qué condiciones se tienen que dar para que un mensaje `M' = H0-0 || H0-1 || H1-0 || H1-1` resulte en el mismo Top Hash?
> 4. Si alguien pudiera encontrar una segunda preimagen de un bloque, podría generar una segunda preimagen del Top Hash. ¿Por qué no representa una debilidad de la primitiva?

**Conceptos:**

* [[Árbol de Merkle]]
* [[Funciones de Hash criptograficas]]

1. Para verificar un bloque individual, alcanza con recalcular `h(bloque 001)` y compararlo con `H0-1`. Pero eso solo no garantiza integridad global — hace falta además subir por el camino hasta el Top Hash usando los "hermanos" del camino (`H0-0`, `H1`) y compararlo con el Top Hash de referencia. Solo se necesita **un hash de hoja** más `O(log n)` hashes del camino, en vez de recalcular todos los bloques.

2. Porque `H1-0` y `H1-1` no son valores de confianza por sí solos: un atacante podría reemplazar los bloques 010 y 011 y recalcular `H1-0'` y `H1-1'` de forma consistente, pasando ambas verificaciones locales sin ser detectado. Solo subiendo hasta el **Top Hash** (el único valor de confianza, el ancla de integridad) se detecta la manipulación, porque el Top Hash recalculado no coincidiría con el guardado.

3. La condición es que el árbol no distinga entre nodos hoja (hash de datos reales) y nodos internos (hash de hashes intermedios): aplica `h()` ciegamente sin saber qué está recibiendo. Esto permite que el atacante presente los hashes intermedios `H0-0 || H0-1 || H1-0 || H1-1` como si fueran los "datos" de un mensaje de 2 bloques, obteniendo el mismo Top Hash. **La solución:** agregar un prefijo distinto según el tipo de nodo — hojas: `h(0x00 || datos)`; nodos internos: `h(0x01 || hijo_izq || hijo_der)` — para que el intercambio ya no produzca el mismo resultado.

4. Si el atacante encuentra un bloque falso con el mismo hash que el original (`h(bloque_falso) = h(Bloque 0)`), toda la cadena hacia arriba se mantiene igual y el Top Hash no cambia — logró una segunda preimagen del Top Hash. Pero esto **no es una debilidad del árbol**, sino de la función de hash subyacente: el Merkle Tree asume que `h` es resistente a segundas preimágenes; si esa premisa no se cumple, el problema es de la función de hash, no de la construcción — análogo a decir que si alguien rompe AES, entonces AES-GCM deja de ser seguro: la debilidad es de AES, no de GCM.
