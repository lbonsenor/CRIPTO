> [!example] Enunciado
> Commitment Scheme — Se utiliza un esquema de compromiso donde:
>
> - **Fase de compromiso:** A envía a B: `h(m)`, donde `h` es una función de hash resistente a colisiones.
> - **Fase de revelación:** A envía a B: `m`. B verifica que `h(m)` coincida con lo recibido anteriormente.
>
> 1. ¿Cómo podría un atacante enviar `h(m)` al principio y después revelar un mensaje `m' ≠ m` sin ser detectado?
> 2. ¿Por qué no se puede usar este esquema para un juego donde alguien saca una carta del 1 al 39 y el otro tiene que adivinar cuál era?
> 3. Si A tiene que emitir una decisión binaria (sí/no), ¿puede la otra parte saber de antemano cuál era la decisión si se envía `h(Enc_k(m))`?

**Conceptos:**

* [[Commitment Scheme]]
* [[Binding]]
* [[Hiding]]
* [[Funciones de Hash criptograficas]]
* [[Nonce]]

1. Para hacer esto, el atacante necesitaría encontrar `m' ≠ m` tal que `h(m') = h(m)` — es decir, una **colisión**. Como `h` es resistente a colisiones, esto es computacionalmente inviable. **No es posible** engañar a B: esto es la propiedad de **[[Binding]]** del esquema.

2. El espacio de mensajes es muy chico. B recibe `h(m)` y sabe que `m` es un número entre 1 y 39: le alcanza con calcular `h(1), h(2), ..., h(39)` y comparar. Esto rompe la propiedad de **[[Hiding]]**. **Solución:** agregar un [[Nonce|nonce]] aleatorio: enviar `h(m || r)`. En la revelación A envía `m` y `r`; ahora B tendría que probar las 39 cartas multiplicado por todos los valores posibles de `r`, lo cual es inviable.

3. Depende de quién conoce la clave `k`:
   - **Si B no conoce `k`:** no puede saber la decisión, aunque el espacio sea chico ("sí"/"no"), porque necesitaría calcular `Enc_k("sí")` y `Enc_k("no")` para comparar, y sin `k` no puede. El cifrado actúa como el nonce del punto anterior. Se mantiene **[[Hiding]]**.
   - **Si B conoce `k`:** puede calcular ambos hashes y comparar — se rompe el hiding, igual que con las cartas del 1 al 39.
