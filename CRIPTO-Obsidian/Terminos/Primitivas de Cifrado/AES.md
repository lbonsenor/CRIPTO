Es el reemplazo de [[Data Encryption Standard (DES)]], orientado a bloques de $128 \text{ bits}$ con una clave de $128$, $192$ o $256 \text{ bits}$

Es la primitiva de cifrado recomendada en la actualidad

>[!example] Esquema de Funcionamiento - 4 Etapas por Ronda
>1. **Byte Sub:** Sustitución de valores según una tabla derivada de invertir una matriz en campo $GF(2^8)$
>2. **Shift Row:** Permutación de bits
>3. **Mix Column:** transformación lineal invertible de cada byte en función de los 4 que forman el  mismo grupo
>4. **Add Round Key**: XOR con la parte de la clave derivada para la ronda en cuestión

>[!info] Variaciones para generar asimetrías
>- **1ra Ronda:** Solo tiene el paso **Add Round Key**
>- **Última Ronda:** No tiene paso **Mix Column**

> [!info] Round Keys (128 bits)
> Los primeros $128\text{ bits}$ corresponden a la clave original
> Para los siguientes:
>> [!example] Generar Máscara
>> - Tomar los últimos $32\text{ bits}$ y rotarlos $8\text{ bits}$ a la derecha
>> - Aplicar **ByteSub** a cada byte
>> - $LSB\ \text{XOR }2^i$
>> 	- $LSB$: Least Significant Byte
>> 	- $i$: El número de grupo de bytes a generar (1 los primeros 16)
>
>> [!example] Generar grupos de $4B$
>> **1ros $4B$**: Mascara XOR key bytes generados 16 posiciones antes
>> **Siguientes:** XOR entre lso 4 bytes generados y los generados 16 posiciones antes
>
>
