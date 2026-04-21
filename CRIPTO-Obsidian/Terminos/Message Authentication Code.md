Es una terna de algoritmos
$\text{Gen}: ()\to K$
$\text{Mac}:K\times P\to T$
$\text{Vrfy}:K\times P\times T\to\{0,1\}$

Tal que:
- $K$ es el espacio de **claves** es el conjunto de todas las claves posibles 
- $P$ es el espacio **plano**, es el conjunto de todos los mensajes posibles 
- $T$ es el espacio **etiquetas**, es el conjunto de todas las etiquetas posibles

> [!info] Propiedad
> $$\forall m,k\text{ validos}:\text{Vrfy}_{k}(m,\text{Mac}_{k}(m))=1$$
