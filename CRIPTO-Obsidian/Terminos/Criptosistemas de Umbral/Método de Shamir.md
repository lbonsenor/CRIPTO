Construcción práctica de un esquema **(t,n)-threshold** basada en interpolación de polinomios. Propuesto por Adi Shamir en 1979. Es la implementación más utilizada de [[Secreto Compartido]].

## Principio matemático

Un polinomio de grado `t-1` queda **completamente determinado** por su evaluación en `t` puntos distintos. Con menos de `t` puntos, el secreto no puede recuperarse (seguridad perfecta — ver [[Secreto Perfecto]]).

## Construcción (distribución del secreto)

Sea `s` el secreto a compartir, y `p` un primo con `p > s` y `p > n`:

1. Definir `a_0 = s`
2. Elegir coeficientes `a_1, ..., a_{t-1}` aleatoriamente en `{0, p-1}`
3. Construir el polinomio:

$$P(x) = a_{t-1} \cdot x^{t-1} + \cdots + a_1 x + a_0 \pmod{p}$$

4. Las **sombras** son: `P(1), P(2), ..., P(n)`

Cada participante i recibe el par `(i, P(i))`.

## Reconstrucción del secreto

Con t sombras cualesquiera `(i_1, s_{i1}), ..., (i_t, s_{it})`:
1. Interpolar el polinomio usando **interpolación de Lagrange**:
$$P(x) = \sum_{j} s_{i_j} \cdot \prod_{k \neq j} \frac{x - i_k}{i_j - i_k} \pmod{p}$$
2. Evaluar `P(0) = s` (el término independiente es el secreto)
## Ejemplo (3,5) — 3 de 5
Secreto: `s = 7`, primo: `p = 11`
Polinomio: `P(x) = 5x² + 3x + 7 mod 11`

|i|P(i)|
|---|---|
|1|4|
|2|0|
|3|6|
|4|2|
|5|4|

Con las sombras `(2,0), (3,6), (5,4)` se puede reconstruir el polinomio y obtener `P(0) = 7`.

## Propiedades

- **Seguridad perfecta**: con menos de T sombras, no se obtiene información sobre el secreto
- **Verificabilidad**: en la variante _VSS (Verifiable Secret Sharing)_ los participantes pueden verificar que sus sombras son correctas
- **Modularidad**: funciona sobre cualquier campo finito

## Ver también
- [[Criptografía de Umbrales]]
- [[Secreto Compartido]]
- [[Secreto Perfecto]]
- [[Funciones de Hash criptograficas]]