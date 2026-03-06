Considerar el alfabeto como un array $A$

Sea un mensaje plano $p$ y un numero $k\in[1,26]$
$\implies \text{Por cada caracter } c\in p: c=A[c+k]$

**Ejemplo**:
$e(\text{prueba}, 4)=\text{tvyife}$
- $\text{p} +4=\text{t}$
- $\text{r} +4=\text{v}$
- $\text{u} +4=\text{y}$
- $\text{e} +4=\text{i}$
- $\text{b} +4=\text{f}$
- $\text{a} +4=\text{e}$

**Debilidad:** Hay pocas claves, se puede adivinar la clave a partir de fuerza bruta hasta que el [[Función de Descifrado|descifrado]] tenga sentido

Informalmente, este cifrado es considerado [[Seguridad|inseguro]]
