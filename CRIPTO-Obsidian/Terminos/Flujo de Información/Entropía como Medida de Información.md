En el contexto del análisis de [[Flujo de Información]], la **entropía** de Shannon se usa para cuantificar la cantidad de información asociada a una variable y detectar si hubo transferencia de información entre variables de un programa.

## Definición

Sea `X` una variable aleatoria discreta que toma valores `x₁ … xₙ`:

$$H(X) = -\sum_{i} p(x_i) \cdot \log_2 p(x_i)$$

Mide la **incertidumbre** a la hora de determinar el valor de `X`.

| Caso | Condición | Entropía |
|---|---|---|
| **Máxima** | Distribución uniforme: `p(xᵢ) = 1/n` | `H(X) = log₂(n)` |
| **Mínima** | Evento único: `p(xᵢ) = 1`, `p(xⱼ) = 0` para `j ≠ i` | `H(X) = 0` |

## Entropía condicional

Sea `X` una VAD con valores `x₁ … xₙ` y `Y` una VAD con valores `y₁ … yₘ`:

$$H(X | Y) = -\sum_{j} p(y_j) \sum_{i} p(x_i | y_j) \cdot \log_2 p(x_i | y_j)$$

`H(X|Y)` mide cuánta incertidumbre queda sobre `X` **después de conocer `Y`**.

## Aplicación: definición formal de flujo

Sea `s` el estado de un sistema y `t` el estado tras ejecutar comandos. Sean `xₛ, yₛ` valores de objetos en `s` e `yₜ` el valor de `y` en `t`:

**Hay flujo de información de `x` a `y` si:**

- Si `y` existe en `s`: `H(xₛ | yₜ) < H(xₛ | yₛ)`
- Si `y` no existe en `s`: `H(xₛ | yₜ) < H(xₛ)`

Es decir: conocer el valor de `y` después de ejecutar los comandos **reduce la incertidumbre sobre `x`**.

## Relación con otros conceptos

- [[Flujo de Información]]: marco donde se aplica esta medición.
- [[Flujo Directo]]: ejemplo concreto de cálculo de entropía.
- [[Flujo Indirecto]]: flujo detectable mediante entropía condicional aun sin asignaciones explícitas.
- [[Control de Flujo de Información]]: técnica que usa este concepto para verificar programas.
