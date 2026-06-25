A partir de una primitiva de cifrado de bloque:
Sea $F$ una función pseudoaleatoria

CBC-MAC (tamaño fijo de mensajes):
$\text{Gen}:k\leftarrow\{0,1\}^n$
$\text{Mac}:$
- Sea $m=m_{1}||m_{2}||m_{3}||\dots||m_{j}$
- Sea $t_{0}=00\dots0$
- $t_{i}=F_{k}(t_{i-1}\oplus m_{i})$
- $\text{Mac}_{k}(m)=t_{j}$
$\text{Vrfy}:\text{Vrfy}_{k}(m,t)=1\iff t=\text{Mac}_{k}(m)$

> [!warning] Propiedad
> Es infalsificable $\iff$ Se permiten solo mensajes de una misma longitud

### Ventajas:

- **Reutilización de componentes:** Si tu sistema ya utiliza un cifrado por bloques (como AES) para la confidencialidad, puedes reutilizar el mismo motor de hardware o biblioteca de software para calcular el MAC. Esto ahorra espacio en sistemas embebidos.
    
- **Seguridad matemática sólida:** Es demostrablemente seguro para mensajes de **longitud fija**.
    

### Desventajas:

- **Vulnerable a mensajes de longitud variable (Ataques de falsificación):** Si se utiliza de forma ingenua con mensajes de distintas longitudes, un atacante puede concatenar mensajes y bloques para forjar un MAC válido sin conocer la clave.
    
    > _Nota:_ Para mitigar esto, se deben usar variantes complejas como **CMAC** o derivar claves adicionales, lo que añade sobrecarga.
    
- **Dependencia del IV (Vector de Inicialización):** El IV **debe ser fijo (usualmente cero)**. Si se usa un IV predecible o aleatorio como en el cifrado estándar, el sistema se vuelve inseguro.