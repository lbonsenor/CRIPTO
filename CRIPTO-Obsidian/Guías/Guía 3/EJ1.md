### $\text{Mac}_{k}(m)=G(k)\oplus m$
Obtengo $f(x)$
Evaluo $00\dots00$, el cual me da $G(k)$ como clave $t$

Por lo tanto, se exactamente cual va a ser la etiqueta $\forall m$ 

**EJEMPLO:**
Asumamos que $G(k)=111111$ (conocido por el adversario)
Por lo tanto se que $\text{Vrfy}_{k}(10011,01100)=1$

$$\therefore\text{Mac-Forge}=1\implies\text{Es falsificable}$$

### $\text{Mac}_{k}(m)=k\oplus firstKBits(m)$
Obtengo $f(x)$
Evaluo $00\dots000$ (una cantidad gigantesca de 0s para asegurarme que $\#0\geq k$), obtengo $t$

Por lo tanto, se que $\text{Vrfy}_{k}(00\dots 0001,t)=1$

$$\therefore\text{Mac-Forge}=1\implies\text{Es falsificable}$$


### $\text{Mac}_{k}(m)=Enc_{k}(|m|)$
Obtengo $f(x)$
Evaluo $0000$, que me retorna la etiqueta $t$

Como $|1111|=|0000|\implies\text{Vrfy}_{k}(1111,t)=1$
$$\therefore\text{Mac-Forge}=1\implies\text{Es falsificable}$$