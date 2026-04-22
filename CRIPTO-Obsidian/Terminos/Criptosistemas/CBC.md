Utiliza la salida de un bloque como entrada para el proximo
- Disminuye el traspaso de información  
- Requiere un valor inicial (IV) ALEATORIO

![](https://www.researchgate.net/profile/Rhouma-Rhouma/publication/215783767/figure/fig1/AS:394138559238144@1470981363092/Cipher-block-chaining-CBC-mode-encryption.png)

> [!info] Seguridad
> CBC es CPA-Secure con IVs aleatorios

> [!danger] Tolerancia a Errores
> Un error en un bloque de cifrado daña el bloque actual y un bit del siguiente.