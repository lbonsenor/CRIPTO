El **salting** es una técnica para reforzar el almacenamiento de [[Claves]] introduciendo una perturbación aleatoria antes de aplicar la función de hash.

## Problema que resuelve

Si un atacante obtiene múltiples valores `c₁, c₂, ..., cₙ` (ej: el archivo `/etc/shadow`), puede calcular `f(aᵢ)` una vez y comparar en paralelo contra todos los `cⱼ`. Esto hace que el costo de atacar N cuentas sea casi igual al de atacar una sola.

## Solución

Introducir una **perturbación aleatoria** `x` (el _salt_) en la función:

```
f(a) = x || f'(a, x)
```

- Cada `c` se calcula con un salt **diferente**.
- El atacante **no puede reutilizar** `f(a)` para buscar en varias cuentas en paralelo.
- Aunque dos usuarios tengan la misma clave, sus `c` serán distintos.

## Propiedades del salt

- Debe ser **único por usuario** (y por cada cambio de clave).
- Se almacena junto al hash resultante (no es secreto, pero es aleatorio).
- Aumenta el costo de los ataques offline proporcionalmente al número de cuentas atacadas.

## Relación con PBKDF2

El salting es un componente de [[PBKDF2]], que extiende la idea añadiendo iteraciones para aumentar el costo computacional de cada prueba.

## Relación con otros conceptos

- [[Claves]]: el activo que protege.
- [[PBKDF2]]: función estándar que incorpora salting e iteraciones.
- [[Ataques a Sistemas de Autenticación]]: el ataque en paralelo que mitiga.
- [[Sistema de Autenticación]]: la función `F` que modifica.
