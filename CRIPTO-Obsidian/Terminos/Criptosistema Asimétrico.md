Es una terna de algoritmos 
$\text{Gen:}()\to PK\times SK$
$\text{Enc}:PK\times P\to C$
$\text{Dec:}SK\times C\to P$

Tal que:
- $PK$ es el conjunto de todas las **claves públicas** posibles
- $SK$ es el conjunto de todas las **claves privadas** posibles 
- $P$, espacio **plano**, es el conjunto de todos los **mensajes** posibles
- $C$, espacio **cifrado**, es el conjunto de todos los **mensajes cifrados** posibles

Un criptosistema es [[Chosen Plain Text indistinguishability|CPA-Secure]]

El nivel de seguridad es relativo a los tamaños de los conjuntos involucrados
- En [[Textbook RSA]]: $n=p\cdot q$
- En [[Elgamal]]: $n=q$

> [!info] Propiedad básica
> $$\forall m,(pk,sk):\text{Dec}_{sk}(\text{Enc}_{pk}(m))=m$$

