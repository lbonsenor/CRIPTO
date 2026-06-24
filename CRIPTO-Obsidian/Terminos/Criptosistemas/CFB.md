Utiliza solo la función $Enc$ 
- Menor complejidad para dispositivos embebidos  
- Permite generar una transformación de flujo a partir de una de bloque

![](https://i.sstatic.net/jaqUc.png)

>[!info] Seguridad
>CFB es CPA-Secure con IVs aleatorios

>[!success] Ventajas
>- **No requiere relleno:** Al funcionar como un cifrado de flujo, puede procesar datos de cualquier tamaño (incluso bit por bit o byte por byte) sin añadir _padding_.
> - **Ideal para flujos de datos en tiempo real:** Útil si necesitas transmitir caracteres individuales a medida que se generan (como en una sesión de terminal SSH).

> [!warning] Desventajas
> - **Secuencial al cifrar:** Al igual que [[CBC]], el cifrado no se puede paralelizar porque depende del output anterior.
>- **Propagación de errores:** Un error en un bloque de texto cifrado afecta el descifrado del bloque actual y de los bloques siguientes hasta que el error "sale" del registro de desplazamiento.