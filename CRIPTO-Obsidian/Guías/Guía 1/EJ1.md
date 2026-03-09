### Cifrado de Rotación
Dado un alfabeto $A$ de longitud $n$ tal que $A_i$ es la posición en el alfabeto ($0\leq i<n$)
Dado un símbolo $c\in A$ y un numero $i\in N$, la operación $c+i=$:
	Sea $k$ un numero tal que $A_{k}=c\implies c+i=A_{k+i}$
Por simpleza:
	Sea $\omega$ una cadena de símbolos $c\in A$, la operación $\omega +i$ es equivalente a realizar la operación de suma sobre cada $c\in\omega$ 

$\text{Gen}: \{0,n\}\to K$
$\text{Enc}: e(p,K)=m+k\mod n$
$\text{Dec}: d(c, K)=c-k\mod n$

### Cifrado de Sustitución Monoalfabetica
Dado dos alfabetos $A, B$ de misma longitud $n$
Dado un símbolo $c\in A$, la operación $A[c]=k$ tal que $A_{k}=c$

$\text{Gen}:\text{Alfabeto } B\to K$
$\text{Enc}: e(p, K)=B_{A[p]}$
$\text{Dec}:d(c,K)=A_{B[c]}$

### Cifrado de Vinegere
Dado un alfabeto $A$
Dado los símbolos $c,d\in A$,  $c+d\equiv c+A[d]$

$\text{Gen}:\text{Subalfabeto B}\subseteq A\wedge \text{long}(B)=n\to K$
$\text{Enc}: e(p, K)=p_{i}+B_{i\%n}$
$\text{Dec}:d(c,K)=c_{i}-B_{i\%n}$

