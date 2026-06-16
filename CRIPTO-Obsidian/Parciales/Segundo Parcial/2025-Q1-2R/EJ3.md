![[2025-Q1-2R-EJ3.png]]
**Conceptos:**
- [[Entropía como Medida de Información]]
- [[Parciales/Imgs/Terminos/Criptosistemas/One Time Pad/Definición|OTP]]

$$H(X) = -\sum_{i} p(x_i) \cdot \log_2 p(x_i)$$
Cadenas binarias de longitud 10. Supongamos que todos los mensajes posibles son equiprobables. La entropía del mensaje H(M) es:
$$H(M)=\log_{2}​(2^{10})=10\text{ bits}$$

Similarmente, como OTP genera una clave aleatoria y uniforme, la entropía de la clave es:
$$H(K)=\log_{2}(2^{10})=10\text{ bits}$$

Usando el teorema de bayes: 
$$\begin{align}
p(c|m)&=p(k=m\oplus c)=\frac{1}{2^{10}} \\
\implies p(m|c)&=\frac{p(c|m)\cdot p(m)}{p(c)}=p(m)
\end{align}$$
Ahora calculemos la entropía condicional:
$$\begin{align}
H(M|C)&=-\sum_{c}p(c)\sum_{m}p(m|c)\cdot \log_{2}p(m|c) \\
&=-\sum_{c}p(c)\sum_{m}p(m)\cdot \log_{2}p(m)=H(M)=10\text{ bits} \\
\implies I(M;C)&=H(M)-H(M|C)=0  \\ \\

& \therefore\text{La cantidad de información que el criptograma revela sobre el mensaje es 0.} 
\end{align}$$

