![](https://miro.medium.com/v2/0*kTGQ9YMgJV_Lw7Qj.png)

>[!info] Seguridad
>CFB es CPA-Secure si no se repite $(k,\text{nonce})$

> [!danger] Tolerancia a Errores
> El error es local; un bit corrupto en el cifrado solo daña un bit en el texto claro

> [!success] Ventajas
> - **Máxima velocidad (Totalmente paralelizable):** Puedes cifrar o descifrar el bloque 100 sin necesidad de procesar los 99 anteriores. Es ideal para aprovechar procesadores multinúcleo modernos.
> - **No requiere relleno:** Al igual que CFB, procesa el tamaño exacto del texto plano.    
> - **Acceso aleatorio:** Puedes descifrar cualquier sección de un archivo cifrado de forma independiente (esencial para sistemas de archivos cifrados o streaming).

>[!warning] Desventajas
>- **Catastrófico si se reutiliza el contador:** Si utilizas el mismo _nonce_ y contador con dos mensajes diferentes bajo la misma clave, un atacante puede aplicar un simple XOR entre los textos cifrados para revelar el texto plano (el mismo peligro que el cifrado One-Time Pad).    
> - **Maleabilidad:** Si un atacante modifica un bit en el texto cifrado, al descifrar se modificará exactamente el mismo bit en el texto plano. No ofrece integridad de datos por sí solo (requiere un MAC adicional o cambiar a un modo moderno como GCM).

