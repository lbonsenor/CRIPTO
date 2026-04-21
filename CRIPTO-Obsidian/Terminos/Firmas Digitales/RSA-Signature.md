Idéntico a [[Textbook RSA]], pero invirtiendo los papeles de las claves:

**Generación de claves**:
- Elegir $p,q\in \mathbb{P}, n=p\cdot q$
- $d\in(0,\Phi(n))\ |\ \text{mcd}(e,\Phi(n))=1$
- Calcular $\frac{d}{e\cdot d}\equiv 1\pmod{\Phi (n)}$
- $pk=(n,e),sk=(n,d)$

$\text{Sign}_{sk}(m)\equiv m^d\pmod{n}$
$\text{Vrfy}_{pk}(m,s)=m=s^e\pmod{n}\color{red}\implies\text{Inseguro}$ 

No es [[Sig-forge]]-Secure
