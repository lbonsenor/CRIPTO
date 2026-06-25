1. A define $\langle{G, q , g}\rangle$, donde:
	1. $G:$ un grupo 
	2. $q:$ el tamaño
	3. $g:$ un generador $g\neq 1\pmod{p}, g^2\neq1\pmod{p},g^q\equiv1\pmod{p}$
2. $A$ elige $x\leftarrow \underbrace{Z_{q}}_{=\{0,1,\dots,q-1\}}$ y calcula $h_{1}=g^x$
3. $A$ envía a $B:(G,q,g,h_{1})$
4. $B$ elige $y\leftarrow Z_{q}$ y calcula $h_{2}=g^y$
5. $B$ envía a $A:(h_{2})$
6. $A$ calcula $k_{a}=h_{2}^x$
7. $B$ calcula $k_{b}=h_{1}^y$

$K_{s}\equiv(g^x)^y\pmod{p}\equiv(g^y)^x\pmod{p}$

> [!info]
> Si $A$ y $B$ se conocen de antemano, pueden tener predefinidos $(G, q, g)$

Esta versión requiere un canal de transmisión autentificado (que un atacante no pueda modificar mensajes)

> [!info] Propiedades
> - Dado $g^x, g^y$, no debería ser posible obtener $x,y$ (Esto se conoce como el problema del logaritmo discreto, y no tiene solución eficiente)
> - Dados $g, g^x, g^y$ , un adversario no puede distinguir $g^{xy}$ de un valor aleatorio

Hoy en día se utiliza para el handshake y distribución de claves