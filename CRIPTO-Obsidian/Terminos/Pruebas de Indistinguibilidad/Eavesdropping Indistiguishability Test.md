Dado un adversario $A(n)$ y un Criptosistema $\Pi(n)$
1. $A$ genera $p_{0}, p_{1}$ arbitrariamente
2. Se genera una clave $k\leftarrow  K$
3. Se genera $b\leftarrow\{0,1\}$
4. $A$ recibe $c=e_{k}(p_{b})$
5. $A$ emite $b'=\{0,1\}$

$EAV_{A,\Pi}=1\iff b=b'$
$P(EAV_{A,\Pi}=1)=0.5+\varepsilon(n)\implies\Pi\text{ es indistinguible}$

$P(b=b')=\sum^n_{i=0\wedge e=1} P(b=b_{i})\cdot P(b'=b_{i}|b=b_{i})$

> [!example] Cifrado [[Por Rotación]]
> 1. $A$ elige $p_{0}=AB, p_{1}=AP$
> 2. Se elige una clave al azar, por ejemplo $k=3$
> 3. Se genera $b=0$
> 4. $A$ recibe $c=DE$
> 5. $A$ emite $b$, como son letras adyacentes, ya sabe que $k=3$ y $b=0$
> Como siempre le voy a atinar, no es indistinguible: $EAV_{A,\Pi}=1$

