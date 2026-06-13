La **fórmula de Anderson** permite estimar la probabilidad de que un atacante adivine una clave, y también calcular el tiempo necesario para lograrlo dado un espacio de claves.

## Fórmula

$$P \approx \frac{T \cdot G}{N}$$

| Variable | Descripción |
|---|---|
| `P` | Probabilidad de adivinar la clave |
| `T` | Tiempo dedicado al ataque (en segundos) |
| `G` | Número de pruebas realizables por segundo |
| `N` | Tamaño del espacio de claves |

Para estimar el **tiempo necesario** para alcanzar probabilidad `P`:

$$T \approx \frac{P \cdot N}{G}$$

## Ejemplos

### Claves numéricas de 10 dígitos, G = 10.000/s, P = 0.5

```
N = 10^10
T = 0.5 × 10^10 / 10.000 = 500.000 s ≈ 6 días
```

### Claves de 8 letras (26 caracteres), G = 10.000/s, P = 0.5

```
N = 26^8 = 208.827.064.576
T = 0.5 × 26^8 / 10.000 ≈ 1.044.135 s ≈ 121 días
```

### ¿Qué longitud mínima para claves alfanuméricas (36 caracteres), G = 100.000/s, P < 0.5 en menos de 1 año?

```
N ≥ T × G / P = (365×24×60×60) × 100.000 / 0.5
N ≥ 6,3 × 10^12

36^L ≥ 6,3 × 10^12
L ≥ log₃₆(6,3 × 10^12) ≈ 8,22  →  L ≥ 9
```

## Uso práctico

Se utiliza para:
- Determinar la longitud mínima de una clave dado un umbral de seguridad.
- Diseñar políticas de [[Claves|expiración y complejidad de claves]].
- Evaluar el impacto de mejoras en hardware de ataque (aumento de `G`).

## Relación con otros conceptos

- [[Ataques a Sistemas de Autenticación]]: contexto donde se aplica.
- [[Claves]]: el activo cuyo espacio se dimensiona con esta fórmula.
- [[Salting]]: reduce la efectividad de ataques en paralelo (reduce el G efectivo).
- [[PBKDF2]]: reduce G al hacer costosa cada evaluación de f.
