Criptosistema donde el espacio de texto plano y el de texto cifrado son idénticos: `P = C = {0, 1, ..., N}`. Permite cifrar un dato (por ejemplo un número de tarjeta de crédito o un código de cupón) de forma que el resultado tenga el mismo formato que el original, sin cambiar la forma en que se almacena o transmite.

Una construcción común a partir de un cifrado en bloque `E_k(x)` de tamaño `n`, para un espacio reducido `m < 2^n`, es aplicar `E_k` repetidamente ("cycle-walking") hasta que el resultado caiga dentro del rango válido:

```
fpe = x
do {
    x = fpe
    fpe = E_k(x)
} while (fpe >= m)
```

Puntos clave para ejercicios:
- La seguridad contra fuerza bruta de la clave depende del tamaño de la clave de `E_k`, no del tamaño del bloque.
- La cantidad de iteraciones necesarias para converger depende de qué tan chico es `m` respecto a `2^n`: si el bloque es mucho más grande que `m`, el algoritmo puede tardar una cantidad inviable de iteraciones en la práctica (trade-off entre seguridad del bloque y practicidad).
- Para verificar si un valor es válido (por ejemplo, si un cupón fue realmente generado por el sistema) alcanza con aplicar el proceso inverso (descifrar con `Dec_k`) y comprobar que el resultado caiga en el rango de valores originalmente usados — no hace falta guardar una lista de valores válidos.
