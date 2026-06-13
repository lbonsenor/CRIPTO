El **flujo directo** ocurre cuando la información se transfiere de una variable a otra a través de una **asignación explícita** en el código.

## Ejemplo

```
y = x + z
```

Con:
- `x` uniforme en `[0..7]` → `H(x) = log₂(8) = 3 bits`
- `z` con distribución: `p(z=1) = 0.5`, `p(z=2) = 0.25`, `p(z=3) = 0.25`

Después de ejecutar la asignación, conociendo `y`, `x` solo puede tomar 3 valores posibles `(y-1, y-2, y-3)`:

$$H(x | y) = -\frac{1}{2}\log_2\frac{1}{2} - 2 \cdot \frac{1}{4}\log_2\frac{1}{4} = 0.5 + 1 = 1.5 \text{ bits}$$

Como `H(x|y) = 1.5 < H(x) = 3` → **hay transferencia de información de `x` a `y`**.

## Interpretación

Conocer el valor de `y` reduce la incertidumbre sobre `x` de 3 bits a 1.5 bits: se transfirieron 1.5 bits de información de `x` hacia `y`.

## Relación con otros conceptos

- [[Entropía como Medida de Información]]: el marco formal para detectar este flujo.
- [[Flujo Indirecto]]: flujo que ocurre sin asignaciones explícitas.
- [[Control de Flujo de Información]]: técnica que detecta ambos tipos de flujo.
