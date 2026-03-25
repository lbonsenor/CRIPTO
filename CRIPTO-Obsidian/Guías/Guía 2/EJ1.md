### a)
$\begin{matrix}&&0.25&0.75\\&&a&b\\ 0.5&k_{1}&1&2\\ 0.25&k_{2}&2&3\\0.25&k_{3}&3&4\end{matrix}$

$P(C=1)=(a\wedge k_{1})=0.25\cdot 0.5=0.125$
$P(C=2)=(b\wedge k_{1})+(a\wedge k_{2})=0.75\cdot 0.5+0.25\cdot 0.25=0.4375$
$P(C=3)=(b\wedge k_{2})+(a\wedge k_{3})=0.75\cdot 0.25+0.25\cdot 0.25=0.25$
$P(C=4)=(b\wedge k_{3})=0.75\cdot 0.25=0.1875$

### b)
#### 1. $P(M=m)=P(M=m|C=c)$

Asumamos que si:
- $P(M=a)=0.25$
- $P(M=a|C=1)=1$
$\implies 0.25=1\to Abs!!$

#### 2. $P(C=c)=P(C=c|M=m)$

Asumamos que si:
- $P(C=1)=0.125$
- $P(C=1|M=b)=0$
$\implies 0.125=0\to Abs!!$

#### 3. $P(C=c|M=m_{0})=P(C=c|M=m_{1})$

Asumamos que si:
- $P(C=1|M=b)=0$
- $P(C=1|M=a)=0.5$
$\implies 0=0.5\to Abs!!$

#### 4. $EAV_{A,\Pi}$

$\begin{matrix}b & k & c & b' & \text{exito} \\ a & k_{1} & 1 & a & 1 \\ a & k_{2} & 2 & b & 0 \\ a & k_{3} & 3 & b & 0 \\ b & k_{1} & 2 & b & 1 \\ b & k_{2} & 3 & b & 1 \\ b & k_{3} & 4 & b & 1\end{matrix}$

$P(\text{exito})=P(b=a|b'=a)+P(b=b|b'=b)$
	$=P(b=a)\cdot P(b'=a|b=a)+P(b=b)\cdot P(b'=b|b=b)$
	$=0.5*P(k_{1}|b=a)+0.5\cdot(P(k_{1}|b=b)+P(k_{2}|b=b)+P(k_{3}|b=b))$
	$=0.5\cdot 0.5+0.5\cdot 1=0.75$