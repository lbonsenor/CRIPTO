> [!note] Enunciado
> El siguiente protocolo usa criptografía de clave pública. Trent tiene una base de datos con todas las claves públicas de los participantes.
> 1. $A \to T: \{A, B\}$
> 2. $T \to A: \{S_T(B, K_{pB}),\ S_T(A, K_{pA})\}$
> 3. $A \to B: \{E_B(S_A(K_s, time_A)),\ S_T(B, K_{pB}),\ S_T(A, K_{pA})\}$
>
> a) Explicar qué hace Bob después del paso 3 para ratificar que puede comunicarse con Alice con seguridad.
> b) Explicar cómo hace Bob para impersonarse como Alice frente a Carol (masquerading).

## a) Verificación de Bob tras el paso 3

Al recibir el mensaje del paso 3, Bob procede así:

1. Usa la clave pública de Trent para verificar $S_T(B, K_{pB})$ y obtener su propia clave pública (confirmando que Trent la certificó).
2. Usa su **clave privada** para desencriptar $E_B(S_A(K_s, time_A))$ y obtener $S_A(K_s, time_A)$.
3. Usa la clave pública de Trent para verificar $S_T(A, K_{pA})$ y obtener la clave pública de Alice.
4. Usa $K_{pA}$ para verificar la firma $S_A(K_s, time_A)$ y obtener $K_s$ y $time_A$.
5. El valor $time_A$ le sirve para detectar posibles ataques de replay.

Bob sabe que solo Alice pudo haber generado $S_A(K_s, time_A)$, y que $K_s$ es la clave de sesión propuesta por Alice.

## b) Bob impersonando a Alice frente a Carol

Una vez que Bob completó el protocolo con Alice, posee $S_T(A, K_{pA})$ (el certificado de Alice firmado por Trent). Bob puede reutilizarlo para iniciar un protocolo con Carol haciéndose pasar por Alice:

$$\begin{align}
1.\quad & B \to T: \{B, C\} \\
2.\quad & T \to B: \{S_T(B, K_{pB}),\ S_T(C, K_{pC})\} \\
3.\quad & B(Alice) \to C: \{E_C(S_A(K_s, time_A)),\ S_T(C, K_{pC}),\ S_T(A, K_{pA})\}
\end{align}$$

Carol recibe el mensaje y verifica la firma de Alice con $K_{pA}$ (certificada por Trent), por lo que se convence de que está hablando con Alice. El protocolo no protege contra este ataque porque Bob tiene acceso al certificado de Alice y al mensaje firmado por ella.
