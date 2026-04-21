Dado un nivel de seguridad $n$, un adversario $A$, y una familia de funciones de hash $H^x(n):$
1. Se selecciona una función $s\leftarrow S$
2. $A$ obtiene acceso a $H(x)=H^s(x)$
3. $A$ emite $x,x'$

$\text{Hash-Coll}_{A,H}=1\iff x\neq x'\wedge H(x)\neq H(x')$
$P(\text{Hash-Coll}_{A,H}=1)<neg(n)\implies H\text{ es libre de colisiones }$