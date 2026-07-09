> [!example] Enunciado
> Sistema de sensores de seguridad en expo de joyas (MALBA) — Una exposición de joyas cuenta con vidrieras, cada una con un sensor de vibraciones. Los sensores se comunican entre sí inalámbricamente con alcance de 2 metros.
>
> Cada sensor envía una señal cada 2 segundos: `Header || id_sensor || estado || id_sensor_extra`, con estados `Stand by`, `Armed`, `Alert`. Cuando un sensor recibe de un vecino un mensaje `armed`/`alert`, cambia su estado y setea `id_sensor_extra`. Si deja de recibir la señal de un vecino por más de 4 segundos, pasa a `alert`. Existe un sensor maestro que inicia la cadena con `armed`. Cada sensor posee un par (sk, pk).
>
> a) Asumiendo que todos los sensores conocen la pk de sus vecinos, ¿cómo modificaría la señal para evitar que un atacante copie el mensaje de un sensor, lo destruya, y siga emitiendo la señal copiada?
> b) Asumiendo que los sensores no conocen inicialmente la pk de sus vecinos, ¿cómo descubrirían esas claves? ¿Se puede validar su integridad?

**Conceptos:**

* [[Firma Digital]]
* [[Replay Attack]]
* [[Ataque Man-In-The-Middle]]
* [[PKI]]

a) **El ataque:** el atacante captura una señal legítima, destruye el sensor, y emite la señal copiada cada 2 segundos. Los vecinos siguen creyendo que el sensor está activo y no disparan `alert`.

   **La solución:** incluir una [[Firma Digital]] y un counter: `Header || id_sensor || estado || id_sensor_extra || CTR || Sign_sk(...)`. Los vecinos verifican la firma con la pk del sensor (el atacante no puede firmar sin la sk destruida junto con el sensor) y que el CTR sea estrictamente mayor al último recibido (anti-[[Replay Attack]]). Se usa counter en vez de timestamp porque los sensores probablemente no tienen reloj sincronizado confiable.

b) **Descubrimiento de claves:** en una fase inicial de setup (antes del estado armed), cada sensor hace broadcast de su pk: `Header || id_sensor || pk_sensor`. Los vecinos almacenan `id_sensor → pk`. Al iniciar la señal `armed` del maestro, termina la fase de descubrimiento y solo se aceptan mensajes firmados.

   **¿Se puede validar la integridad de las pk recibidas?** No, no de forma confiable: sin una [[PKI]] ni conocimiento previo de las claves, un atacante podría posicionarse dentro del rango durante el setup y hacer broadcast de su propia pk haciéndose pasar por un sensor legítimo — un [[Ataque Man-In-The-Middle|ataque de masquerading]] en la fase de descubrimiento. La mitigación parcial es que el setup ocurra en un momento controlado (instalación) y que luego no se acepten nuevas pk.
