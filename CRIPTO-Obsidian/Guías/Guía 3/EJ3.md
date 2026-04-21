### a)
[[Funciones de Hash criptograficas#^1b1caf|Niveles de Seguridad]]

**Resistencia a preimágenes**
Solo hay dos $y$ posibles, 111...11 y 000...0, y ambos los puedo encontrar con $x=1$ y $x=11$ respectivamente

**Resistencia a segundas imágenes**
$11\neq1111$, pero $h(11)=h(1111)$

**Resistencia a colisiones**

### b)
$$P(h(x_{1})=h(x_{2}))=\underbrace{P(x_{1}\equiv 0(2)\cap x_{2}\equiv0(2))}_{0.25}+\underbrace{P(x_{1}\equiv 1(2)\cap x_{2}\equiv1(2))}_{0.25}=0.5$$

