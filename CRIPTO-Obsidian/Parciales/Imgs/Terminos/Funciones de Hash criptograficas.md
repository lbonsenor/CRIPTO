Son pares de algoritmos:
$\text{Gen}: s\leftarrow S$
$\text{Hash}:h=H^{s}(m)\in{0,1}^{L}$

Tal que:
- $s$ es un selector (en muchas implementaciones $S=\{s_{0}\}$)
- $L$ es la longitud del hash

Son análogas a los [[Message Authentication Code]], pero sin clave

>[!info] Propiedad
>**Resistencia a preimágenes**
>> $\forall y$ es computacionalmente imposible hallar $x:h(x)=y$
>
>**Resistencia a segundas imágenes**
>> $\forall x$ es computacionalmente imposible hallar $x'\neq x:h(x')=h(x)$
>
>**Resistencia a colisiones**
>> $\forall x,x':x\neq x'$ Es computacionalmente hallar $x,x':h(x)=h(x')$

^1b1caf

### Colisión
$$x\ne x'\wedge h(x)=h(x')$$
>[!warning] Propiedad
> Si existen $n+1$ mensajes y $n$ valores de salida $\implies \exists\text{Colision }$

### Seguridad
$$\text{Sea }h:A\to B$$
##### Preimágenes
Dado $y\in B$, hallar $x:h(x)=y$
Es una búsqueda por fuerza bruta, requiere $B$ intentos

##### Segundas imágenes
Dado $(x,y):h(x)=y$, hallar $x':h(x')=y$
Es una búsqueda por fuerza bruta, requiere $B$ intentos

##### Colisiones
Hallar $x,x':h(x)=h(x')$
Es una búsqueda por fuerza bruta, requiere $B^{\frac12}$ intentos