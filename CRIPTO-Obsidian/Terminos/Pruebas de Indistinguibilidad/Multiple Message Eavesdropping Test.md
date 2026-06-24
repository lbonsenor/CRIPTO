Dado un adversario $A$, y un Criptosistema $\Pi$
1. $A$ genera ($m_{00}, m_{01}, \dots,m_{0i}$) y ($m_{10}, m_{11}, \dots,m_{1i}$)
2. Se genera una clave $k\leftarrow K$
3. Se genera $b\leftarrow\{0,1\}$
4. A recibe ($c_{0},c_{1},\dots,c_{i}$) donde $c_{j}=Enc_{k}(m_{bj})$
5. A emite $b'=\{0,1\}$

$Mul_{A,\Pi}=1\iff b=b'$
$P(Mul_{A,\Pi}=1)=0.5+\varepsilon\implies \Pi \text{ es indistinguible}$

>[!info] Propiedad
>No hay ninguna primitiva de cifrado que pase la prueba

> [!example] Cifrado [[OTP]]
> 1. $A$ genera $(\text{HOLA}, \text{CHAU})$ y $(\text{HOLA}, \text{HOLA})$
> 2. Se genera una clave $k$
> 3. Se genera $b=1$
> 4. $A$ recibe $(\text{XYZ, XYZ})$
> 5. Como son iguales, se que $b=1$, y siempre voy a atinar, por lo tanto no es indistinguible