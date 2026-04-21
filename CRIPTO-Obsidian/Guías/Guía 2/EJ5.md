### [[Terminos/Criptosistemas/Por Sustitución/Definición|Por Sustitución]]

1. Genero una clave $k$
2. Genero $m_{0}=abcd, m_{1}=aaaa$
3. Genero $b$
4. Recibo $c=XXXX\vee c=WXYZ$
5. Emito $b'$

Se que si $c$ tiene el mismo carácter, es $m_{1}$, caso contrario, $m_{0}$
$$\implies CPA_{A, \Pi}=1$$
$$\therefore \text{No es seguro}$$

### [[Terminos/Criptosistemas/Vigenere/Definición|Por Vigenere]]

1. Genero una clave $k$
2. Genero $m_{0}=abcd,m_{1}=aaaa$, lo que me da $c_{0}=WXYZ$, $c_{1}=STUV$
3. Genero $b$
4. Recibo $c=WXYZ\vee c=STUV$
5. Emito $b'$

Se que si $c=WXYZ$, entonces se trata de $m_{0}$
