> [!note] Enunciado
> En el protocolo original de Needham-Schroeder, cuando se roban claves de sesión es posible un ataque de replay. La siguiente es una variante del protocolo:
> 1. $Alice \to Bob: Alice$
> 2. $Bob \to Alice: \{ Alice, rand_X \}_{K_{BT}}$
> 3. $Alice \to Trent: \{ Alice, Bob, rand_A, \{ Alice, rand_X \}_{K_{BT}} \}$
> 4. $Trent \to Alice: \{ Alice, Bob, rand_A, K_{session}, \{ Alice, rand_X, K_{session} \}_{K_{TB}} \}_{K_{AT}}$
> 5. $Alice \to Bob: \{ Alice, rand_X, K_{session} \}_{K_{TB}}$
> 6. $Bob \to Alice: \{ rand_B \}_{K_s}$
> 7. $Alice \to Bob: \{ rand_B - 1 \}_{K_s}$
>
> Mostrar que con esta variante se resuelve el problema de ataque de repetición.

## Ataque en el protocolo original

Si Mallory consigue una clave de sesión vieja $K_{session}$, puede enviarle a Bob un mensaje viejo $\{Alice, K_{session}\}_{K_{TB}}$. Bob lo abre con su clave compartida con Trent, obtiene $K_{session}$ y continúa el protocolo creyendo que habla con Alice. Como Bob no inició el protocolo, no puede darse cuenta de que el mensaje es viejo.

## Por qué la variante lo resuelve

En esta variante, el mensaje del paso 5 incluye el nonce $rand_X$ generado por Bob:

$$Alice \to Bob: \{ Alice, rand_X, K_{session} \}_{K_{TB}}$$

**Caso 1: Mallory intenta el replay sin que Bob haya iniciado el protocolo.**

Mallory envía directamente el paso 5 con un mensaje viejo. Bob, al abrirlo, ve que no envió recientemente ningún mensaje del paso 2 con ese $rand_X$, por lo que **rechaza el mensaje**.

**Caso 2: Mallory intenta el replay después de que Bob inició el protocolo.**

Bob generó recientemente $rand_X'$ (fresco) en el paso 2. El mensaje viejo de Mallory contiene un $rand_X \neq rand_X'$, por lo que **Bob también lo rechaza**.

**Caso 3: Mallory inicia el protocolo haciéndose pasar por Alice.**

$$\begin{align}
1.\quad & Mallory(A) \to Bob: Alice \\
2.\quad & Bob \to Mallory(A): \{ Alice, rand_X' \}_{K_{BT}} \\
3.\quad & \text{Mallory no puede abrir este mensaje (no tiene } K_{BT} \text{ ni } K_{AT}\text{)} \\
5.\quad & Mallory(A) \to Bob: \{ Alice, rand_X, K_{session} \}_{K_{TB}} \quad \text{(mensaje viejo)}
\end{align}$$

Bob abre el mensaje y ve que $rand_X \neq rand_X'$, por lo tanto **no continúa el protocolo**.

En todos los casos, la inclusión de $rand_X$ (generado por Bob en cada sesión) dentro del ticket cifrado con $K_{TB}$ impide que Mallory pueda reutilizar mensajes viejos.
