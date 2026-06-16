Es un criptosistema basado en [[Intercambio Diffie-Hellman]]
**Generación de claves:**
- Seleccionar $G,q,g$
- $x \in Z_{q}, h=g^x$
- $pk=(G,q,g,h), sk=(G,q,g,x)$

$\text{Enc}_{pk}(m):y\in Z_{q}\to c=(c_{1},c_{2})=(g^y,h^y\cdot m)$
$\text{Dec}_{sk}(c)=\left( \frac{c_{2}}{c_{1}} \right)^x$

Si la prueba de decisión DH es difícil en $G$, es [[Chosen Plain Text indistinguishability|CPA-Secure]]

