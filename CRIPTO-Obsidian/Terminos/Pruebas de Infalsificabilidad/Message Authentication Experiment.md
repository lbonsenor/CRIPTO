Dado un nivel de seguridad $n$, un adversario $A$, y un criptosistema $\Pi(n)$
1. Se genera una clave $k\leftarrow K$
2. $A$ obtiene $f(x)=\text{Mac}_{k}(x)$
3. $A$ realiza las evaluaciones que quiera de $f(x)$
4. Sea $Q$ al conjunto de evaluaciones, $A$ emite $(m,t)$ donde $m\not\in Q$

$\text{Mac-Forge}_{A,\Pi}=1\iff\text{Vrfy}_{k}(m,t)=1$
$P(\text{Mac-Forge}_{A,\Pi}=1)\leq neg(n)\implies\Pi\text{ es infalsificable}$

> [!warning] Observaciones
> 1. El adversario puede obtener un MAC para cualquier mensaje que elija 
> 2. Se considera roto el MAC si el adversario puede falsificar cualquier mensaje, independientemente de si tiene sentido o no. 


La construcción [[HMAC]] es infalsificable