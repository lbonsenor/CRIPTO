Dado un adversario $A$, y un Criptosistema $\Pi$
1. Se genera una clave $k\leftarrow K$
2. $A$ obtiene $f(x)=e_{k}(x)$ y genera $(m_{0},m_{1})$
3. Se genera $b\leftarrow\{0,1\}$
4. $A$ recibe $c=e_k(m_b)$
5. $A$ emite $b'=\{0,1\}$

$CPA_{A,\Pi}=1\iff b=b'$
$P(CPA_{A,\Pi}=1)=0.5+\varepsilon\implies\Pi \text{ es indistinguible}$

>[!note] Propiedades
>- Un criptosistema determinístico no puede ser CPA-Secure
>- Si un criptosistema es CPA-Secure para un mensaje también lo es para múltiples
>- Un criptosistema que es CPA-Secure, pero de tamaño limitado (cifra mensajes de hasta n bits) puede ser extendido arbitrariamente

A diferencia de [[Eavesdropping Indistiguishability Test]], este adversario tiene acceso a la función $\text{Enc}(k)$
