> [!note] Enunciado
> Considera el protocolo de intercambio de claves Diffie-Hellman y escribe la secuencia de pasos para que en lugar de ser 2 los participantes que generan una clave compartida sean 3.

Dados $(g, p)$ públicos, y claves privadas $x, y, z$ de A, B y C respectivamente:

$$\begin{align}
1.\quad & A \to B: g^x \mod p = X \\
2.\quad & B \to C: g^y \mod p = Y \\
3.\quad & C \to A: g^z \mod p = Z \\
4.\quad & A \to B: (g^z)^x \mod p = Z' \\
5.\quad & B \to C: (g^x)^y \mod p = X' \\
6.\quad & C \to A: (g^y)^z \mod p = Y'
\end{align}$$

Después de esto, A, B y C calculan la clave compartida $k$:

$$\begin{align}
A&: (Y')^x \mod p = (g^{yz})^x \mod p = g^{xyz} \mod p = k \\
B&: (Z')^y \mod p = (g^{xz})^y \mod p = g^{xyz} \mod p = k \\
C&: (X')^z \mod p = (g^{xy})^z \mod p = g^{xyz} \mod p = k
\end{align}$$

Los tres participantes obtienen la misma clave $k = g^{xyz} \mod p$.
