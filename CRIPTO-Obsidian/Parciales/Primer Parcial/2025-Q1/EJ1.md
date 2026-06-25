![[2025-Q1-EJ1.png]]
**Conceptos**
- [[Intercambio Diffie-Hellman]]
### a) 
Es un protocolo de **acuerdo o intercambio de claves** (_Key Exchange / Key Agreement_).

Su objetivo es permitir que dos partes (A y B), que se comunican a través de un canal público e inseguro (donde un atacante puede escuchar todo lo que se envían), construyan y compartan un **secreto común algebraico** (kA​=kB​). Al finalizar con éxito, ambas partes tienen la misma clave simétrica privada, la cual pueden usar posteriormente para cifrar sus mensajes reales (por ejemplo, usando AES).

### b)
**q debe ser un número primo grande.** Si q no fuera primo, la estructura algebraica se rompe, no todos los elementos tendrían inversos multiplicativos y el subgrupo generado podría ser muy pequeño y fácil de predecir.

**(1.1)** $A$ le envía a $B$: el grupo definido por $q=23$ y el generador $g=5$.
**(1.2 y 1.3)** $A$ elige en secreto $x=6,x\in \mathbb{Z}_{23}$
	$h_{1}=g^x\pmod{q}\implies h_{1}=8$
**(1.4)** $A\to B$: Envía $h_{1}​=8$.
**(1.5 y 1.6)** $B$ elige en secreto $y=15$.
	$h_{2}=g^y\pmod{q}\implies h_{2}=19$
	$B\to A$: Envía $h_{2}=19$
**(1.7)** $A$ calcula su clave:
	$k_{A}=(h_{2})^x\pmod{q}\implies k_{A}=2$
**(1.8)** B calcula su clave:
	$k_{A}=(h_{1})^y\pmod{q}\implies k_{b}=2$

### c)
La seguridad de este protocolo se basa fundamentalmente en la dureza matemática del **Problema del Logaritmo Discreto (DLP)** y, más específicamente, en el **Problema de Diffie-Hellman (DHP)**.

### d)
- **Vulnerabilidad al ataque Man-in-the-Middle (MitM):** El protocolo **no provee autenticación**. Un atacante activo posicionado en el medio de la red puede interceptar h1​ y mandarle a B su propio valor inventado. Lo mismo puede hacer de regreso con B. Al final, el atacante terminaría acordando una clave secreta con A y otra con B, pudiendo descifrar, leer y modificar todo el tráfico sin que ninguno de los dos lo note.
    
- **Vulnerabilidad a ataques de subgrupos pequeños (si los parámetros son malos):** Si el primo q no se elige con cuidado (por ejemplo, si no es un _Primo de Safe_ de la forma q=2p+1 donde p también es primo), el orden del grupo puede factorizarse en números pequeños. Un atacante podría forzar a que las claves caigan en un subgrupo algebraico muy reducido, reduciendo exponencialmente el espacio de búsqueda para adivinar la clave por fuerza bruta.