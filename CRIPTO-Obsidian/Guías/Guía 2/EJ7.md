### A)
Son de 32 bits por el $\text{mod }32$

### B)
![](https://www.researchgate.net/profile/Rhouma-Rhouma/publication/215783767/figure/fig1/AS:394138559238144@1470981363092/Cipher-block-chaining-CBC-mode-encryption.png)

---
$24\oplus 19=11$
$(11*7)\text{ mod } 32 = 13$
---
$((17\oplus 13)\cdot 7)\text{ mod 32}=4$
---
$((26\oplus 4)\cdot 7)\text{ mod 32}=18$
---
$((25\oplus 18)\cdot 7)\text{ mod 32}=13$
---
$((12\oplus 13)\cdot 7)\text{ mod 32}=7$

$$\therefore c=13\ 4\ 18\ 13\ 7$$
