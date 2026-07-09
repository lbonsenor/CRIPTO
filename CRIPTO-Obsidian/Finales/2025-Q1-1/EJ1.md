> [!example] Enunciado
> Sistema de transporte de valores — Una empresa de transporte de valores utiliza el siguiente sistema para monitorear objetos de valor en tránsito:
>
> - Cada camión tiene un **controlador** atornillado al vehículo.
> - Cada ítem de valor va acompañado de un **emisor** que debe estar en contacto físico con el objeto.
> - Los emisores y el controlador se comunican mediante **Bluetooth** (cualquiera que escuche esa frecuencia puede ver los mensajes).
> - Cada emisor envía periódicamente la trama: `BEACON | id_emisor | CTR | Enc_k(status)`, donde `status` indica si el emisor está o no en contacto físico con el objeto, `CTR` es un contador y `Enc_k` es un cifrado con clave compartida entre el emisor y el servidor.
> - El controlador del camión, al detectar una trama `BEACON`, la captura y la retransmite a un servidor central.
>
> 1. ¿Cómo puede darse cuenta el servidor de que se llevaron el ítem de valor y dejaron el emisor? ¿Y si se llevan las dos cosas?
> 2. ¿Cómo puede un atacante con acceso a un dispositivo Bluetooth robar un ítem sin que el servidor se entere? ¿Cómo se podría solucionar?
> 3. Si un atacante emite un mensaje con bytes arbitrarios, el servidor dispara alarmas falsas. ¿Qué validación extra podría tener el servidor para evitar esto?

**Conceptos:**

* [[Replay Attack]]
* [[Message Authentication Code]]
* [[Chosen Ciphertext Attack]]
* [[GCM - Galois Counter Mode]]

1. **Se llevan el ítem y dejan el emisor:** el emisor detecta que ya no está en contacto y envía `status = sin contacto`; el servidor dispara alarma. **Se llevan ambos:** el emisor sale del rango Bluetooth del controlador, que deja de recibir sus tramas; el servidor detecta la ausencia de señal y dispara alarma.

2. **El ataque:** el atacante escucha el Bluetooth (público), captura una trama legítima con `status = contacto`, se lleva el ítem, y con su propio dispositivo reenvía la trama capturada repetidamente ([[Replay Attack]]). El controlador la retransmite y el servidor cree que todo está bien.

   **La solución:** el servidor guarda el último CTR recibido de cada emisor y exige que cada nuevo CTR sea estrictamente mayor. El CTR debe ir tanto en plano como dentro del cifrado: `BEACON | id_emisor | CTR | Enc_k(status || CTR)`. El servidor lee el CTR en plano, verifica que sea mayor, descifra y compara el CTR interno con el externo para detectar manipulación. El atacante no puede modificar el CTR interno porque está cifrado con una clave que no conoce.

3. **La solución:** agregar un [[Message Authentication Code|MAC]] que cubra **toda la trama**: `BEACON | id_emisor | CTR | Enc_k(status || CTR) | MAC_k'(BEACON || id_emisor || CTR || Enc_k(status || CTR))`. La clave del MAC (k') debe ser distinta a la de cifrado para mantener [[Chosen Ciphertext Attack|CCA-security]], o alternativamente reemplazar todo el esquema por [[GCM - Galois Counter Mode|AES-GCM]]. El servidor verifica el MAC antes de procesar cualquier trama; si no es válido, la descarta silenciosamente sin disparar alarma.
