> [!example] Enunciado
> Commitment Scheme con XOR y PRG — Un protocolo permite a A elegir un valor sin mostrárselo a B, y luego revelarlo:
>
> - **Setup:** A elige una clave k (nueva por mensaje) y genera `r = G(k)` con un generador pseudoaleatorio G.
> - **Compromiso:** A elige `m`, calcula `c = m ⊕ r`, y envía `c` a B.
> - **Revelación:** A envía `(m, r)`. B acepta si `c = m ⊕ r`.
>
> a) Si B tiene `c`, ¿puede conocer `m` o parte de `m`?
> b) ¿Puede A falsificar un mensaje `m' ≠ m` en la revelación sin que B lo detecte?
> c) ¿Cómo modificaría el protocolo para que en la revelación se envíe `k` en lugar de `r`? ¿Qué efecto tiene sobre la falsificación?

**Conceptos:**

* [[Commitment Scheme]]
* [[Hiding]]
* [[Binding]]
* [[OTP]]

a) No. `c = m ⊕ r` con `r = G(k)` pseudoaleatorio; sin conocer `k` ni `r`, B no puede recuperar información sobre `m`. Es análogo al [[OTP|OTP]]: el XOR con un valor pseudoaleatorio oculta completamente el mensaje. Se mantiene **[[Hiding]]**.

b) Sí. A puede elegir cualquier `m' ≠ m` y calcular `r' = c ⊕ m'`. B verifica `m' ⊕ r' = m' ⊕ (c ⊕ m') = c` y la verificación pasa. El esquema **no tiene [[Binding]]** cuando se revela `r`, porque para cualquier `m'` existe un `r'` válido.

c) Se modifica la revelación para enviar `(m, k)` en vez de `(m, r)`; B calcula `r = G(k)` y verifica `c = m ⊕ G(k)`. Esto **fortalece el binding**: para falsificar, A necesitaría encontrar `k'` tal que `G(k') = c ⊕ m'`, pero G no es invertible — no se puede hallar la entrada que produce una salida deseada. A tendría que probar `k'` al azar, lo cual es computacionalmente inviable. **Revelar `k` en vez de `r` le da binding al esquema.**
