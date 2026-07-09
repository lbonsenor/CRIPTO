> [!example] Enunciado
> Protocolo para que N nodos elijan un supervisor sin que ninguno pueda manipular sistemáticamente el resultado:
>
> 1. Cada nodo genera `r_i` de 64 bits y envía `H(r_i)` al resto.
> 2. Luego de recibir todos los valores, cada nodo publica `r_i`.
> 3. Cada nodo calcula `r_x = r_1 ⊕ r_2 ⊕ ... ⊕ r_n`.
>
> El nodo a menor distancia de Hamming de `r_x` es el supervisor (empate → se reinicia).
>
> 1. Explicar por qué el paso 1 es necesario y no se puede empezar directamente con el paso 2.
> 2. Con tres nodos, ¿qué pasaría si el nodo 2 repite `H(r_1)` como propio valor? Proponer una modificación simple.
> 3. Proponer una mejora para que no sea necesario repetir todo el intercambio en caso de empate.

**Conceptos:**

* [[Commitment Scheme]]
* [[Funciones de Hash criptograficas]]

1. Es un [[Commitment Scheme|commitment scheme]]: el hash de los `r_i` originales asegura que ningún nodo pueda cambiar su valor luego. Empezando directamente por el paso 2, un nodo podría esperar a ver los `r_i` de los demás y elegir el propio para que el `r_x` resultante quede lo más cerca posible de su identificador (probando distintos valores propios hasta encontrar el que produzca la menor distancia de Hamming a su favor).

2. Si el nodo 2 publica `H(r_1)`, entonces `r_2 = r_1` y `r_x = r_1 ⊕ r_2 ⊕ r_3 = r_3`. El nodo 2 sabe de antemano que `r_x = r_3` y puede manipular la elección con ese dato. **Modificación:** incluir el identificador del nodo dentro del hash: enviar `H(r_i || i)` en vez de `H(r_i)`. En la revelación, los demás verifican `H(r_i || i)` usando el identificador real. Si el nodo 2 copia `H(r_1 || 1)`, en la revelación no puede presentar `r_1` (los demás calculan `H(r_1 || 2) ≠ H(r_1 || 1)`) ni encontrar un `r'` tal que `H(r' || 2) = H(r_1 || 1)` sin romper la resistencia a colisiones.

3. En caso de empate, comparar los bits de prefijo de a uno (`n-1`, `n-2`, ... `1` bit) usando distancia de Hamming hasta sacar un ganador, en vez de reiniciar todo el intercambio, asegurando además que los hashes publicados en el primer paso sean distintos entre sí (por resistencia a colisiones, un mismo hash no puede provenir de dos mensajes distintos).
