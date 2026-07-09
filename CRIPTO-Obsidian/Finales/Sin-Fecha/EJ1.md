> [!example] Enunciado
> El sistema de seguridad del museo Odyssey está automatizado. Cada pieza tiene una etiqueta de 1mm² que es un sensor de seguridad (radio ~1 metro). Al activarse, cada sensor registra a sus vecinos y repite la medición cada 5 segundos; si hay discrepancia, emite alerta. La alerta se retransmite de vecino en vecino hasta un panel en cada habitación conectado a la alarma central. Los sensores tienen par de claves (sk, pk) de curvas elípticas de 320 bits, provistas de fábrica, sin conocerse entre sí a priori.
>
> La versión inicial transmitía en plano. Para cada problema encontrado, proponer una modificación que NO deshaga la seguridad conseguida por las anteriores:
>
> 1. Fue posible reemplazar un elemento con su sensor por un transmisor que emite las mismas señales tras escuchar al sensor unos minutos.
> 2. Fue posible desactivar el sistema transmitiendo una señal de apagado.
> 3. Fue posible disparar la alarma con un transmisor que emita la señal que los sensores interpretan como alarma.

**Conceptos:**

* [[Firma Digital]]
* [[Replay Attack]]
* [[PKI]]

1. **El ataque:** el atacante capturó una señal y hace [[Replay Attack|replay]] con un transmisor propio. **La solución:** que cada señal incluya un counter y vaya firmada: `id_sensor || CTR || Sign_sk(id_sensor || CTR)`. Al activarse el sistema, cada sensor hace broadcast de su pk a sus vecinos (`id_sensor || pk_sensor`) para poder verificar firmas después (con la limitación de que, sin [[PKI]], no se puede validar la integridad de esas pk durante el setup inicial — la mitigación es que ese setup ocurra en un momento controlado, al instalar el sistema, y no se acepten nuevas pk después). Cada vecino verifica la firma con la pk registrada y que el CTR sea estrictamente mayor al último recibido de ese sensor.

2. **El ataque:** el atacante transmite una señal de apagado y los sensores obedecen. **La solución:** con las firmas del punto 1, los sensores solo aceptan señales de sensores vecinos conocidos. Además, la señal de apagado debería ser un comando especial que solo el panel central puede emitir, firmado con su propia sk (que los sensores conocen desde el setup): `id_panel || "APAGAR" || CTR || Sign_sk_panel(...)`. Un atacante no puede falsificar la firma del panel, y un replay de una señal vieja falla por el CTR.

3. **El ataque:** el atacante transmite una señal de "ALERTA" falsa que se retransmite en cadena hasta el panel. **La solución:** cada sensor, al recibir una alerta válida (firmada) de un vecino, **no la retransmite tal cual**, sino que genera su propia alerta firmada con su propia sk: `id_vecino || "ALERTA" || CTR_vecino || Sign_sk_vecino(...)`. Así cada sensor solo necesita verificar la firma de su vecino inmediato, y un atacante externo no puede inyectar una alerta en ningún punto de la cadena porque no tiene la sk de ningún sensor legítimo.
