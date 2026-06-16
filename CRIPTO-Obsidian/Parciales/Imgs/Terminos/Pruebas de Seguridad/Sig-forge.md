Dado un nivel de seguridad $n$, un adversario $A$, y una [[Firma Digital]] $\Pi(n)$:
1. Se generan claves $(sk,pk)\in K$
2. $A$ obtiene $f(x)=\text{Sign}_{sk}(x)$ y $pk$
3. $A$ realiza las evaluaciones que quiera de $f(x)$ ($Q=$ conjunto de esas evaluaciones)
4. $A$ emite $(m,s)$, donde $m\not\in Q$

$\text{Sig-forge}_{A,\Pi}=1\iff\text{Vrfy}_{pk}(m,s)=1\wedge m\not\in Q$
