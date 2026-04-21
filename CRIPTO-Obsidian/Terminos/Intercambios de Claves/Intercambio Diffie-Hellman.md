1. A define $\langle{G, q , g}\rangle$, donde:
	1. $G:$ un grupo 
	2. $q:$ el tamaño  
	3. $g:$ un generador
2. $A$ elige $x\leftarrow \underbrace{Z_{q}}_{=\{0,1,\dots,q-1\}}$ y calcula $h_{1}=g^x$
3. $A$ envía a $B:(G,q,g,h_{1})$
4. $B$ elige $y\leftarrow Z_{q}$ y calcula $h_{2}=g^y$
5. $B$ envía a $A:(h_{2})$
6. $A$ calcula $k_{a}=h_{2}^x$
7. $B$ calcula $k_{b}=h_{1}^y$

$K_{s}\equiv(g^x)^y\quad(p)\equiv(g^y)^x\quad(p)$

> [!info]
> Si $A$ y $B$ se conocen de antemano, pueden tener predefinidos $(G, q, g)$

Esta versión requiere un canal de transmisión autentificado (que un atacante no pueda modificar mensajes)