> [!note] Enunciado
> ¿Cuál es el problema con el siguiente protocolo? Solucionarlo.
> 1. $A \to B: S_A\{N_1, K_s\}$
> 2. $B \to A: \{N_1 + 1\}_{K_s}$

## Problema

Cualquiera que tenga la clave pública de A puede obtener $N_1$ y $K_s$ del primer mensaje, ya que $S_A\{\cdot\}$ es una firma con clave privada de A verificable con su clave pública. El protocolo no garantiza confidencialidad, solo identidad del firmante.

## Solución

Combinar encripción con firma: Alice firma el mensaje con su clave privada **y** lo encripta con la clave pública de Bob:
$$\begin{align}
A \to B&: E_B\{S_A\{N_1, K_s\}\} \\
B \to A&: \{N_1 + 1\}_{K_s}
\end{align}$$

- Solo Bob puede desencriptar con su clave privada y obtener $S_A\{N_1, K_s\}$.
- Con la clave pública de Alice, Bob verifica la firma y obtiene $\{N_1, K_s\}$, sabiendo que solo Alice pudo haberlo generado.
