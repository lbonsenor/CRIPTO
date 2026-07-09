Protocolo de dos fases entre A y B:

1. **Commit**: A se compromete con un mensaje `m` sin revelarlo (por ejemplo enviando `h(m)` usando una [[Funciones de Hash criptograficas|función de hash]], o `c = m ⊕ G(k)` con un PRG).
2. **Reveal**: A revela `m` (y eventualmente los valores auxiliares) y B verifica que corresponde al compromiso original.

Un esquema de compromiso debe cumplir simultáneamente dos propiedades: [[Hiding]] (B no puede aprender `m` antes del reveal) y [[Binding]] (A no puede cambiar `m` después de comprometerse). La solidez de cada propiedad depende de la construcción elegida:

- `h(m)` con espacio de mensajes chico → falla Hiding (fuerza bruta). Se soluciona agregando un nonce aleatorio: `h(m || r)`.
- `c = m ⊕ r`, revelando `r` → falla Binding (A puede elegir un `r'` que le convenga). Se soluciona revelando una semilla `k` que fuerza a invertir un PRG en vez de despejar un XOR.
- `h(Enc_k(m))` → seguro en ambas propiedades siempre que B no conozca `k`.

Casos de uso típicos: sorteos (bingo), juegos de cartas, votaciones, y cualquier escenario donde una parte deba decidir "a ciegas" antes de que la otra se comprometa.
