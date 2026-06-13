El **flujo indirecto** ocurre cuando la información se transfiere entre variables **sin que aparezcan juntas en una asignación explícita**. Puede ocurrir mediante estructuras de control o comportamiento del programa.

## Flujo por estructuras de control

```python
if (x == 0):
    y = 1
else:
    y = 0
```

`x` e `y` no aparecen en la misma asignación, pero:

Con `x ∈ {0, 1}` y `p(x=0) = 0.5`:

- `H(x) = 1 bit`
- `H(x | y) = 0` (conocer `y` revela `x` completamente)

→ **Hay transferencia total de información de `x` a `y`.**

## Flujo por comportamiento (terminación)

```c
while (x == 0) {}
```

No existe ninguna asignación. Sin embargo:

Definir `y = 0` si el programa termina, `y = 1` si no termina. Con `x ∈ {0, 1}` y `p(x=0) = 0.5`:

- `H(x) = 1 bit`
- `H(x | y) = 0` (la terminación del programa revela el valor de `x`)

→ **Hay transferencia de información incluso sin ninguna asignación.**

## Implicaciones

El flujo indirecto es especialmente difícil de detectar porque no es visible en las asignaciones del código. Las técnicas de [[Control de Flujo de Información]] en tiempo de compilación deben contemplarlo explícitamente.

## Relación con otros conceptos

- [[Entropía como Medida de Información]]: framework para detectarlo formalmente.
- [[Flujo Directo]]: la contraparte con asignaciones explícitas.
- [[Control de Flujo de Información]]: técnica que analiza ambos tipos.
- [[Covert Channel]]: el flujo indirecto por comportamiento es la base de muchos canales encubiertos.
