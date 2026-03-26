Es una sustitución polialfabética, el cual es una mezcla entre el cifrado por [[Por Rotación|Rotación]] y por[[Terminos/Criptosistemas/Por Sustitución/Definición|Sustitución]]

Es una clave compuesta por $n$ números, tal que la posición $i$ en el texto plano se rota por $K[i\mod(n)]$

**Ejemplo:**

$\text{Sea la clave } K=4253$
$\text{Sea el texto plano p=esto es una prueba}$

```
esto| es |una |prue|ba
4253|4253|4253|4253|42
iuyr|dgxc|yofc|ttzh|fc
```
$$\implies d(p,k)=\text{iyurdgxcyofcttzhfc}$$
Como se puede ver, el primer carácter de $p$ se rota por 4, el segundo por 2, y así sucesivamente

Idealmente, la clave tiene mayor o igual longitud que el texto plano

**Ataque:**
- Determinar la longitud del bloque
- Analizar la clave de cada bloque por separado
- Aparecen $n$-gramas repetidos al transformar los mismos símbolos en la misma posición' 
- Buscar secuencias repetidas
- Calcular la distancia entre las secuencias
- La longitud de la clave es múltiplo del MCD entre las distancias halladas


