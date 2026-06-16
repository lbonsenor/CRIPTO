A partir de una primitiva de cifrado de bloque:
Sea $F$ una función pseudoaleatoria

CBC-MAC (tamaño fijo de mensajes):
$\text{Gen}:k\leftarrow\{0,1\}^n$
$\text{Mac}:$
- Sea $m=m_{1}||m_{2}||m_{3}||\dots||m_{j}$
- Sea $t_{0}=00\dots0$
- $t_{i}=F_{k}(t_{i-1}\oplus m_{i})$
- $\text{Mac}_{k}(m)=t_{j}$
$\text{Vrfy}:\text{Vrfy}_{k}(m,t)=1\iff t=\text{Mac}_{k}(m)$

> [!warning] Propiedad
> Es infalsificable $\iff$ Se permiten mensajes de una misma longitud

