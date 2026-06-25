Generación de Claves:
- Se elige $p,q\in \mathbb{P}, n=p\cdot q$
- $e\in(0,\Phi (n))\ |\ \text{mcd}(e, \Phi(n))=1$
- Calcular $\frac{d}{e\cdot d}\equiv 1\pmod{\Phi(n)}$
- $pk=(n,e)$
- $sk=(n,d)$

$\text{Enc}_{pk}(p)\equiv p^e\pmod{n}$
$\text{Dec}_{sk}(c)=c^d\pmod{n}$

>[!danger] Problemas
>- Si $e$ y $m$ son pequeños: $m^e < n$, y entonces se puede calcular el logaritmo
>- Si dos pares de claves comparten el mismo n, es posible recuperar n (y luego la clave privada) 

>[!example] Ejemplo
>1. Elegimos $p=61, q=53$, $n=3233$
>2. $\Phi(n)=(p-1)(q-1)=3120$. Elegimos $e=17$
>3. ${17\cdot d}\equiv 1\pmod{3120}\implies d=2753$
>4. $pk=(3233, 17)$
>5. $sk=(2753,n)$
