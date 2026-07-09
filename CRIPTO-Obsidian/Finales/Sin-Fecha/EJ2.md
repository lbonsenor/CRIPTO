> [!example] Enunciado
> **Format Preserving Encryption (FPE):** `P = C = {0,1,...,N}` para un N arbitrario. Dado `E_k(x)` una primitiva de cifrado en bloque de tamaño `n`, se define `FPE_k(x)` con `P = C = {0,...,m-1}`, `m < 2^n`:
>
> ```
> fpe = x
> do {
>   x = fpe
>   fpe = E_k(x)
> } while (fpe >= m)
> ```
>
> Un uso es generar cupones de 20 dígitos cifrando los números del 0 al 9.999 (10.000 cupones).
>
> 1. ¿Cuál es la probabilidad de que un atacante genere un cupón válido?
> 2. ¿Cambia en algo la seguridad si se cifran los números del 10.000 al 19.999 en lugar de los originales?
> 3. ¿Varía el esfuerzo del atacante si se construye el FPE con claves de 64, 128 o 256 bits?
> 4. Dado un cupón de 20 dígitos, ¿cómo se puede verificar si es válido?

**Conceptos:**

* [[Format Preserving Encryption (FPE)]]
* [[Fuerza Bruta]]

1. El atacante tiene dos caminos: **romper la clave** (probabilidad `1/2^n` para una clave de `n` bits) o **adivinar un cupón al azar**: el espacio de cupones de 20 dígitos es `10^20`, y hay 10.000 cupones válidos, así que la probabilidad es `10.000 / 10^20 = 1/10^16`.

2. No, porque [[Format Preserving Encryption (FPE)|FPE]] es una biyección sobre el espacio completo; cifrar 0-9.999 o 10.000-19.999 produce la misma cantidad de cupones válidos (10.000) distribuidos en el mismo espacio. La probabilidad de adivinar sigue siendo `1/10^16`.

3. Depende del ataque: si intenta romper la clave por [[Fuerza Bruta]], sí varía (`2^64` vs `2^128` vs `2^256`). Si adivina cupones al azar, no varía — la probabilidad de acertar es `1/10^16` independientemente del tamaño de la clave.

4. Aplicar el proceso inverso (`Dec_k`, con el mismo mecanismo de iteración) al cupón y verificar que el resultado esté en el rango original de valores usados (0 a 9.999). No hace falta guardar ni buscar en una lista de cupones válidos.
