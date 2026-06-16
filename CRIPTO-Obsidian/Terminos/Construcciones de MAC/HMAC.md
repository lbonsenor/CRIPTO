Dado $H(x)$ una función de hash libre de colisiones

$HMACk(x)$
- $\text{Gen}:k\leftarrow K,s\leftarrow S$
- $\text{Mac}: t=H^s((k\oplus \text{opad})||H^{s}((k\oplus\text{ipad})||m))$
- $\text{opad}=0x36\dots36$
- $\text{ipad}:0x 5c 5c\dots 5c$

