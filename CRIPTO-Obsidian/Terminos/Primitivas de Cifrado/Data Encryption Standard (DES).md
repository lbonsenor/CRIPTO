$\text{Entrada: } 64\text{ bits}$
$\text{Clave: } 64\text{ bits (56 efectivos)}$
$\text{Salida: } 64\text{ bits}$

1. Se parte el mensaje en dos mitades
2. Se transforma una mitad con parte de la clave
3. Se intercambian las dos mitades de lugar
4. Se repiten los pasos 1,2 y 3 (una ronda) 16 veces

![400](https://upload.wikimedia.org/wikipedia/commons/thumb/f/fa/Feistel_cipher_diagram_en.svg/960px-Feistel_cipher_diagram_en.svg.png)

- **Por fuerza bruta:** $2^{56}$ intentos
- **Con criptoanálisis diferencial:** $2^{47}$ intentos ^9033b0
- **Con Criptoanálisis lineal:** $2^{43}$ intentos