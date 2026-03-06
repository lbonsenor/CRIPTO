Un **criptosistema** es una terna de algoritmos

$\text{Generador de Clave: } ()\to K$
$\text{Cifrado: }K\times P\to C$
$\text{Descifrado: }K\times C\to P$

Tal que:
- $K$ es el **espacio de claves**: el conjunto de todas las *claves* posibles
- $P$ es el **espacio plano**: el conjunto de todos los *mensajes* posibles
- $C$ es el **espacio cifrado**: el conjunto de todos los *mensajes cifrados* posibles 

> [!Note] Propiedad Basica: Función Inversa
> $$\forall m,k\text{ validos}:d_{k}(e_{k}(m))=m$$ 


