> [!example] Enunciado
> Un oleoducto transporta petróleo a grandes distancias con un sistema de control activo. Cada 200m hay sensores conectados por cable interno; cada 10km hay concentradores (dispositivos externos) conectados a segmentos anterior y posterior, que transmiten por radiofrecuencia a una central.
>
> Escenario de ataque: un grupo toma control físico de un concentrador para manipularlo y "pinchar" el tubo, extrayendo petróleo en pequeñas cantidades.
>
> Cada grupo de sensores tiene un identificador público y uno privado de 128 bits (nunca transmitido). Los sensores transmiten `(identificador, enc(valor_sensado))` usando **CHACHA20**. Los concentradores capturan mensajes de ambas secciones durante 1 minuto y los envían como `(identificador, msg1|...|msgn, mac(msg1|...|msgn))` usando **HMAC-SHA3** con su identificador privado.
>
> 1. ¿Qué debe hacer la central para verificar la integridad de los valores de cada sensor antes de determinar si son normales?
> 2. ¿Puede alguien con capacidad de reemplazar un concentrador (con su identificador privado) falsificar valores que la central da como válidos?
> 3. ¿Puede alguien con capacidad de reemplazar dos concentradores hacerlo?

**Conceptos:**

* [[ChaCha20]]
* [[HMAC]]
* [[Bit-Flipping Attack]]

1. **(1)** Verificar el MAC del concentrador con [[HMAC]]-SHA3 usando el identificador privado que la central conoce, descartando el mensaje si no es válido. **(2)** Descifrar cada `(identificador_sensor, enc(valor))` con [[ChaCha20]] usando la clave del sensor correspondiente, verificando que el identificador exista y sea legítimo para ese segmento. **(3)** Validar que los valores descifrados sean físicamente plausibles (rangos posibles, ningún sensor faltante, consistencia con vecinos), ya que ChaCha20 es un cifrado de flujo y no provee integridad por sí solo — la central no puede garantizar criptográficamente que los valores no fueron manipulados, solo detectar inconsistencias groseras.

2. **Sí.** El atacante, con el identificador privado del concentrador, puede calcular un HMAC-SHA3 válido sobre cualquier mensaje — el MAC deja de proteger. Además, aunque no conozca las claves de los sensores individuales, puede hacer **[[Bit-Flipping Attack|bit-flipping]]** sobre el ciphertext de ChaCha20 (`ciphertext = plaintext ⊕ keystream`): modificar bits específicos del cifrado produce cambios predecibles en el descifrado, permitiendo llevar valores anómalos (como una caída de presión) a rangos que parezcan normales, todo mientras recalcula el MAC del concentrador con su identificador robado.

3. **Sí, y es peor.** Con un solo concentrador comprometido, la central podría comparar los valores del mismo sensor reportados por los dos concentradores vecinos (cada sensor es capturado por ambos). Con **dos concentradores consecutivos comprometidos**, el atacante controla ambos "testigos" de los sensores entre ellos y puede manipular consistentemente ambos reportes — la central pierde su único mecanismo de verificación cruzada para esos sensores.
