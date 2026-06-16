![[2025-Q1-2R-EJ4.png]]
**Conceptos:**
- [[Fórmula de Anderson]]
- [[SHA-1]]
### a)
$$\begin{align}
P&=\frac{T\cdot G}{N} \\
\frac{1}{100}&=\frac{(365\cdot 24\cdot 60\cdot 60)\text{ segs}\cdot(10^5)\text{ c/s}}{96^n} \\
96^n&=3,1536\times 10^{14} \\
n&=\log_{96}(3,1536\times 10^{14})\approx7,314
\end{align}$$

Por lo tanto, $n=\lceil 7.314 \rceil=8$

### b)
**No, no es valido ni seguro**
Si un atacante puede probar $10^5$ contraseñas por segundo, considerando que SHA-256 es una función criptográfica de hash diseñada para que sea extremadamente veloz, esto deja que atacantes puedan testear miles de millones de combinaciones con GPUs modernas. 