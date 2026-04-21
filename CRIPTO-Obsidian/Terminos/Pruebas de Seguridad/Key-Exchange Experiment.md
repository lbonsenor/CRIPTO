Dado un adversario $A$, y una primitiva de [[Intercambio de Claves]] $\Pi$:
1. Se ejecuta $\Pi$, sea $k=k_{a}=k_{b}$
2. Se genera $b\leftarrow\{0,1\}$
3. Si $b=0\implies k'\leftarrow\{0,1\}^n$, Si $b=1 \implies k'=k$
4. $A$ obtiene $\text{Trans}$ y $k'$, y emite $b'=\{0,1\}$

$KE_{A,\Pi}=1\iff b=b'$
$P(KE_{A,\Pi}=1)<0.5+\varepsilon\implies\Pi\text{ es seguro}$