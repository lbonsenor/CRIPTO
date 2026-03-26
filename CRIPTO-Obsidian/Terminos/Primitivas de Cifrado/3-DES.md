Es la variante fuerte de [[Data Encryption Standard (DES)]]

Consiste en seleccionar 3 claves independientes y calcular
$c=Enc_{k_{1}}(Dec_{k_{2}}(Enc_{k_{3}}(p)))$

Debido al ataque **meet-in-the middle**, en vez de tener una seguridad de orden $168 \text{ bits}$, tiene una de $112 \text{ bits}$

- Es inmune a [[Data Encryption Standard (DES)#^9033b0|criptoanálisis diferencial y lineal]]
- Es 3 veces más lenta que DES