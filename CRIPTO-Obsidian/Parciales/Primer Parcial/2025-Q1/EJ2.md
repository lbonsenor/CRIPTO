![[Pasted image 20260624031709.png]]

**Conceptos:**
- [[CBC]]
- [[Counter (CTR)]]
- [[OFB]]
### a)
Siempre y cuando $E_{k}$ tenga una funcion de desencripción $D_{k}$, es válido por ser reversible
$$\begin{align}
C_{i}&=E_{k}(M_{i})\oplus C_{i-1} \\
M_{i}&=D_{k}(C_{i}\oplus C_{i-1})
\end{align}$$

### b)

|                       | Confidenciabilidad    | Propagación de errores                                       |
| --------------------- | --------------------- | ------------------------------------------------------------ |
| **Esquema propuesto** | No, pues filtra el IV | Se propaga exactamente a **2 bloques** ($M_{i}, M_{i+1}$)    |
| **CBC**               | Si, con IV aleatorio  | Destruye por completo $M_{i}$ y afecta bit a bit a $M_{i+1}$ |
| **CTR**               | Si, con nonce único   | No se propaga, solo afecta al bit correspondiente            |
| **OFB**               | Si, con IV aleatorio  | No se propaga, solo afecta al bit correspondiente            |
