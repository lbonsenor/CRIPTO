Errores que restan puntos en los ejercicios de protocolos y pentesting:

- Decir "replay attack" sin explicar cómo, a quién y qué efecto tiene.
- Confundir [[Modelo Bell-La Padula]] (confidencialidad) con [[Modelo Biba]] (integridad).
- Decir que [[CBC]] o [[ChaCha20]] dan integridad — no dan, solo confidencialidad.
- Proponer un MAC con la misma clave que el cifrado — no es [[Chosen Ciphertext Attack|CCA-secure]].
- Decir que un hash "protege" sin especificar qué propiedad (preimagen, colisión, segunda preimagen).
- Escribir hipótesis genéricas de pentesting ("puede haber XSS") sin decir dónde ni cómo.
- No vincular la hipótesis al enunciado — siempre citar componentes específicos del sistema descripto.
- Confundir la clave privada del usuario con la del proveedor de identidad — en [[SSO - Single Sign-On|SSO]] la clave privada relevante suele ser la del IdP, no la del usuario.
- Decir que el atacante "no puede modificar" un mensaje solo porque está cifrado — cifrado sin integridad permite [[Bit-Flipping Attack|bit-flipping]].
- No explicar por qué la solución funciona — siempre cerrar con "el atacante no puede X porque Y".
