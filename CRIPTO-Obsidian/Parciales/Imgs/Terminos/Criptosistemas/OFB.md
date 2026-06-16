Construye una transformación de flujo a partir de  una de bloque 
- Permite calcular los bits de la transformación por adelantado

![](https://www.researchgate.net/profile/Drdinesh_Goyal/publication/273260684/figure/fig4/AS:391769960271878@1470416645589/OFB-Output-Feedback-mode-Counter-CTR-Mode-Counter-mode-is-a-stream-cipher-such-as.png)

>[!info] Seguridad
>OFB es CPA-Secure si no se repite $(k,\text{nonce})$

> [!danger] Tolerancia a Errores
> El error es local; un bit corrupto en el cifrado solo daña un bit en el texto claro