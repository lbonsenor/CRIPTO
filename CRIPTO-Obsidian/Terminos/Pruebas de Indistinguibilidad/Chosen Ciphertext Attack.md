Dado un nivel de seguridad $n$, un adversario $A$, y un criptosistema $\Pi(n)$
1. Se genera una clave $k\leftarrow K$
2. $A$ obtiene $f(x)=e_{k}(x)$ y $g(x)=d_{k}(x)$, y se genera $(m_{0},m_{1})$
3. Se genera $b\leftarrow\{0,1\}$
4. $A$ recibe $c=e_{k}(m_{b})$, y no puede calcular $g(c)$
5. $A$ emite $b'=\{0,1\}$

$CCA_{A,\Pi}=1\iff b=b'$
$P(CCA_{A,\Pi}=1)< \frac{1}{2}+neg(n)\implies\Pi\text{ es indistinguible}$

Un criptosistema CCA-Secure permite enviar información manteniendo confidencialidad e integridad, pero requiere que ambas partes conozcan la misma clave

Como las claves no pueden transmitirse por un canal inseguro, se usa un canal seguro (trusted third party). Existe una clave por cada combinación, y cada parte debe administrar $n-1$ claves