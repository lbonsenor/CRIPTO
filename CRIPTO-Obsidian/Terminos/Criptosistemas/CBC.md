Utiliza la salida de un bloque como entrada para el proximo
- Disminuye el traspaso de información  
- Requiere un valor inicial (IV) ALEATORIO

![](https://www.researchgate.net/profile/Rhouma-Rhouma/publication/215783767/figure/fig1/AS:394138559238144@1470981363092/Cipher-block-chaining-CBC-mode-encryption.png)

> [!info] Seguridad
> CBC es CPA-Secure con IVs aleatorios

> [!danger] Tolerancia a Errores
> Un error en un bloque de cifrado daña el bloque actual y un bit del siguiente.

> [!success] Ventajas
> - **Alta confidencialidad:** Bloques de texto plano idénticos se cifran en bloques de texto cifrado completamente diferentes (gracias a la dependencia del bloque anterior).
> - **Estándar de la industria:** Está ampliamente soportado y ha sido el caballo de batalla de la criptografía durante décadas.

> [!warning] Desventajas
> - **No es paralelizable al cifrar:** Como necesitas el resultado del bloque anterior para cifrar el siguiente, el cifrado debe ser estrictamente secuencial (lento en procesadores modernos). _(Nota: El descifrado sí se puede paralelizar)._
> - **Propagación de errores:** Si un bit se corrompe en la transmisión, arruina por completo el bloque actual y cambia un bit del bloque siguiente al descifrar.    
> - **Requiere relleno (Padding):** El texto plano total debe ser un múltiplo exacto del tamaño del bloque. Si no lo es, hay que rellenarlo, lo que abre la puerta a ataques como el _Padding Oracle Attack_.

