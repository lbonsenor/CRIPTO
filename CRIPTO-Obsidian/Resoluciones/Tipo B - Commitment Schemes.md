Ejemplos: hash commitment, XOR/PRG commitment, bingo, cartas, decisiones binarias.

## Método de resolución

**1. Identificar las dos propiedades de un [[Commitment Scheme]]:**
- [[Binding]]: ¿puede A cambiar su mensaje después del commit?
- [[Hiding]]: ¿puede B descubrir el mensaje antes del reveal?

**2. Para cada propiedad, testear:**
- ¿El espacio de mensajes es chico? → B puede hacer fuerza bruta → [[Hiding]] roto.
- ¿A puede elegir libremente qué revelar? → Si revela `r` directamente en un esquema XOR, puede falsificar el mensaje comprometido.
- ¿A tiene que invertir algo computacionalmente difícil para cambiar el mensaje? → Si revela una semilla `k` y B calcula `G(k)`, [[Binding]] se fortalece.

**3. Soluciones típicas:**
- Espacio chico → agregar un [[Nonce|nonce]] aleatorio: `h(m || r)`.
- Binding débil → revelar `k` en vez de `r` (forzar a invertir un PRG).
- Cifrado como nonce → `h(Enc_k(m))` funciona si B no conoce `k`.

## Tabla de variantes

| Esquema                        | Hiding                  | Binding                      | Problema / solución          |
| ------------------------------ | ----------------------- | ---------------------------- | ---------------------------- |
| `h(m)` con espacio grande      | ✅                       | ✅ (resistencia a colisiones) | Ninguno                      |
| `h(m)` con espacio chico       | ❌ Fuerza bruta          | ✅                            | Agregar nonce: `h(m \|\| r)` |
| `c = m ⊕ r`, revelando `r`     | ✅ (si r es aleatorio)   | ❌ A elige `r' = c ⊕ m'`      | Revelar `k` en vez de `r`    |
| `c = m ⊕ G(k)`, revelando `k`  | ✅                       | ✅ (no invertible)            | Ninguno                      |
| `h(Enc_k(m))`, B no conoce `k` | ✅                       | ✅                            | Ninguno                      |
| `h(Enc_k(m))`, B conoce `k`    | ❌ (calcula ambos lados) | ✅                            | No dar `k` a B               |
