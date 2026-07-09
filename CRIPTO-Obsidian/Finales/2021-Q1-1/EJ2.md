> [!example] Enunciado
> Algoritmo para que dos sistemas se sincronicen en un valor (0 o 1) sin que ninguno pueda influenciarlo:
>
> 1. A genera `r1` y envía a B `H(r1)`.
> 2. B genera `r2` y envía a A `H(r2)`.
> 3. A envía `r1` a B.
> 4. B envía `r2` a A.
>
> A y B calculan `x = (r1 & 1) xor (r2 & 1)`.
>
> a) Explicar por qué son necesarios los pasos 1 y 2. ¿Qué verificación adicional deberían hacer A y B?
> b) Suponer que B recibe `H(r1)` y reenvía ese valor a A en el paso 2. ¿Qué conseguiría? Proponer una modificación.
> c) Modificar el algoritmo para que A y B se pongan de acuerdo en el valor de un byte en lugar de un bit.

**Conceptos:**

* [[Commitment Scheme]]
* [[Funciones de Hash criptograficas]]

a) Los pasos 1 y 2 funcionan como un **[[Commitment Scheme|commitment scheme]]**: A se compromete a `r1` sin que B pueda conocerlo (hash no invertible), y viceversa. Recién en los pasos 3 y 4 se revelan los valores reales. **Verificación faltante:** al recibir `r2` (paso 4), A debe calcular `H(r2)` y verificar que coincida con lo recibido en el paso 2 (y análogamente B con `r1`). Si no coincide, la otra parte cambió su valor después de ver el ajeno — detectado por **resistencia a segundas preimágenes**.

b) Entonces `x = (r1 & 1) xor (r1 & 1) = 0` siempre, sin importar qué elija A: B controló el resultado sin conocer `r1`. **Modificación:** incluir la identidad del emisor dentro del hash: `A envía H("A" || r1)`, `B envía H("B" || r2)`. Si B reenvía `H("A" || r1)` como propio, en el reveal tendría que presentar un `r2` tal que `H("B" || r2) = H("A" || r1)` — inviable por resistencia a colisiones.

c) Reemplazar la máscara de 1 bit por una de 8 bits: `x = (r1 & 0xFF) xor (r2 & 0xFF)`, donde `0xFF = 11111111` extrae los 8 bits menos significativos de cada valor, dando un byte entre 0 y 255.
