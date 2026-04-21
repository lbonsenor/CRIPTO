**RSA Labs Public Key Cryptography Standard** define una versión con padding aleatorio de RSA:
- Sea $k$ la longitud de $n$ en bytes
- Solo se permiten cifrar mensajes de hasta $n-11$ bytes
- Sea $D$ la longitud de $m$ en bytes
- $m'=00000000||0000010||r||00000000||m$
- Donde $r=k-D-3$ bytes aleatorios $\neq_{0}$

Se cree que es [[Chosen Plain Text indistinguishability|CPA-Secure]], pero no esta comprobado que no es [[Chosen Ciphertext Attack|CCA-Secure]]
