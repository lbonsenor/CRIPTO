Dado un [[Criptosistema]] $C=\langle\text{Gen},e,d\rangle$

El criptosistema posee **secreto perfecto** si para toda distribución de probabilidades en $M$, $\forall m, \forall c\text{ tal que }P(C=c)=0:$ 
$$P(M=m|C=c)=P(M=m)$$
> [!Warning] Paradoja del Secreto Perfecto:
> Notar que $c=e_{k}(m)$, así que las variables aleatorias discretas $C$ y $E$ son dependientes. Sin embargo, la propiedad de secreto perfecto interpretada probabilísticamente dice que son V.A.D.s independientes.

