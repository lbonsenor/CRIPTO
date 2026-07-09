> [!example] Enunciado
> Se quiere ocultar un mensaje de forma segura con una técnica similar a las sombras. Dado un mensaje `m` se eligen dos números random (x1, x2) y se generan por dos canales separados:
>
> - `p1 = x1 xor m xor x2`
> - `p2 = x2 xor x1`
> - Canal 1: `p1` y `h(p2)`
> - Canal 2: `p2` y `h(p1)`
>
> 1. Explicar por qué si un atacante tiene acceso a solo 1 canal aún se mantiene la confidencialidad.
> 2. Explicar por qué si un atacante tiene acceso a solo 1 canal aún se mantiene la integridad.
> 3. Si un atacante tiene acceso a los 2 canales, ¿cómo hacer para que se mantenga solo la integridad?

**Conceptos:**

* [[Funciones de Hash criptograficas]]
* [[HMAC]]
* [[Secreto Compartido]]

1. Con acceso al canal 1: tiene `p1` y `h(p2)` — no puede sacar `p2` porque la función hash no es invertible, y tampoco puede sacar `m` de `p1` porque no conoce los dos números random `x1` y `x2` con los que está xoreado. Lo mismo aplica simétricamente al canal 2. La confidencialidad se mantiene porque el hash no revela información y ambas `p` están xoreadas por dos números random.

2. Con acceso al canal 1, si alguien falsifica el valor de `p1`, al recibirse `h(p1)` (por el canal 2) se detecta la discrepancia por la propiedad de **resistencia a segundas preimágenes** (no existe `p1' ≠ p1` tal que `h(p1') = h(p1)`).

3. Reemplazar el hash por **[[HMAC]]** con una clave secreta `k` compartida: `Canal 1: p1 y HMAC_k(p2)` / `Canal 2: p2 y HMAC_k(p1)`. El atacante con ambos canales puede calcular `m = p1 xor p2` (la confidencialidad se rompe, pero la consigna pide mantener solo integridad). Si quiere modificar `p1 → p1'`, necesitaría recalcular `HMAC_k(p1')` para el canal 2, y no puede porque no conoce `k`.
