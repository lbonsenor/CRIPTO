$\text{Gen: } k\leftarrow\{0,1\}^n, n=|p|$
$\text{Enc: }e_{k}(p)=p\oplus k$
$\text{Dec: }d_{k}(c)=c\oplus k$

**Ejemplo:**
```
msg = 0
key = r

c =   01001010010110100
```

**Propiedad fundamental:** Tiene secreto perfecto, mientras que la clave sea aleatoria

$\forall C=\langle G,E,D\rangle\text{ con secreto perfecto}\iff C\equiv OTP$

Ante la redefinición del [[Seguridad Computacional|secreto perfecto]], OTP se redifine a
$\text{Gen: } k\leftarrow\{0,1\}^n, n=|p|$
$\text{Enc: }e_{k}(p)=p\oplus G(k)$
$\text{Dec: }d_{k}(c)=c\oplus G(k)$
