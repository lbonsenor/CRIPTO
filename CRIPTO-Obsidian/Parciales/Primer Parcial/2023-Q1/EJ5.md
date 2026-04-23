![[Parciales/Primer Parcial/2023-Q1/Images/EJ5.png]]

a) Para proveer privacidad e integridad, lo correcto es utilizar el esquema **Encrypt-then-MAC**: primero cifrar m con k1​ para obtener c, y luego calcular el MAC de c con k2​ para obtener t. Tanto **c** como t se deben transmitir en forma conjunta. Las claves k1​ y k2​ **deben ser diferentes**.

b) La seguridad de las funciones de hash se establece por su nivel de resistencia a preimágenes, a **segundas preimágenes** y, fundamentalmente, a **colisiones** (donde es computacionalmente difícil hallar dos entradas distintas x1​=x2​ tales que h(x1​)=h(x2​)).

c) El algoritmo de Diffie-Hellman permite que Alice y Bob **acuerden (o establezcan)** una clave de sesión a través de un canal inseguro, sin que ninguna de las partes tenga que enviársela a la otra.

d) En los cifrados en bloque, **dependiendo** del modo de operación, puede no ser necesario que la función sea reversible. En modos como **CTR (Counter)** o **OFB**, el algoritmo se utiliza únicamente para generar un flujo de clave (_keystream_) que se aplica con XOR, por lo que tanto para cifrar como para descifrar se usa la función de cifrado en sentido directo.