**Generación de claves:**
- Seleccionar $H(x)$ [[SHA-1]] o SHA-2
- Tamaño de claves: $(L,N): (1024, 160), (2048, 224), (2048, 256)\text{ o }(3072, 256)$
- $q\in \mathbb{P}\text{ de tamaño N en bits}$
- $p\in \mathbb{P}\text{ de tamaño P}$
	- $p-1\equiv 0\pmod{q}$
- $g\in\text{Gen de orden }q\pmod{p}$
	- $g^{(p-1)/q}\neq {1}$
- Seleccionar $p,q,g (G=Z^*_{p})$
- $x \in Z_{q}, y=g^x\pmod{p}$
- $pk=(p,q,g,y),sk=(p,q,g,x)$

$\text{Sign}_{sk}(m):k\in Z_{q},r=(g^k\pmod{p})\pmod{q}$
	$s=[H(m)+x\cdot r]\cdot k^{-1}\pmod{q}$
	$\text{Sign}_{sk}(m)=(r,s)$

$\text{Vrfy}_{pk}(m,(r,s)):v_{1}=[H(m)\cdot s^{-1}]\pmod{q}$
	$v_{2}=r\cdot s-1\pmod{q}$
	