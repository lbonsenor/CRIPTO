> [!note] Enunciado
> Considera el siguiente protocolo Diffie-Hellman con autenticación:
> 1. $A \to B: g^x$
> 2. $B \to A: \{B, cert_B, S_B(g^x, g^y), g^y\}$
> 3. $A \to B: \{A, cert_A, S_A(g^x, g^y)\}$
>
> a) Explicar el por qué de las firmas en el protocolo.
> b) Mostrar que un atacante activo Mallory puede realizar un ataque man in the middle tal que Alice cree comunicarse con Bob, pero Bob cree comunicarse con Mallory.

## a) Rol de las firmas

Las firmas garantizan **autenticidad** de los valores Diffie-Hellman:

- $S_B(g^x, g^y)$ garantiza que fue **Bob** quien recibió $g^x$ y generó $g^y$.
- $S_A(g^x, g^y)$ garantiza que fue **Alice** quien generó $g^x$ y recibió $g^y$.

Sin firmas, cualquier intermediario podría sustituir los valores $g^x$ o $g^y$ sin que las partes lo detecten.

## b) Ataque Man in the Middle de Mallory

$$\begin{align}
1.\quad & A \to M \text{ (captura M)}: g^x \\
2.\quad & M \to B: g^x \\
3.\quad & B \to M: \{B, cert_B, S_B(g^x, g^y), g^y\} \\
4.\quad & M(B) \to A: \{B, cert_B, S_B(g^x, g^y), g^y\} \\
5.\quad & A \to M \text{ (captura M)}: \{A, cert_A, S_A(g^x, g^y)\} \\
6.\quad & M \to B: \{M, cert_M, S_M(g^x, g^y)\}
\end{align}$$

La firma que hace Mallory en el paso 6 es posible porque pudo ver en plano los valores $g^x$ y $g^y$ en los pasos 1 y 3.

**Resultado:**
- Alice cree comunicarse con Bob (recibió el certificado y firma legítimos de Bob en el paso 4).
- Bob cree comunicarse con Mallory (recibió el certificado de Mallory en el paso 6).
- Mallory comparte una clave $g^{xy}$ con Alice y otra con Bob, pudiendo interceptar toda la comunicación.
