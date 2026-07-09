> [!example] Enunciado
> La empresa Pavota SAS ofrece sistemas de control de entradas para eventos (recitales, partidos, etc.) Uno de sus sistemas utiliza tickets impresos o digitales con un código QR.
>
> El QR de cada entrada contiene la siguiente información: `(event_id, tipo_de_entrada, #ticket, mac)`, donde `event_id` es un identificador único por evento, `tipo_de_entrada` indica la zona (palco, vip, campo, etc.), `#ticket` es un número único que identifica a cada ticket, `mac = mac_{k_evento}(event_id || #ticket)`. Finalmente `k_evento` es una clave simétrica de 128 bits definida para cada evento, conocida por el servidor y los lectores de QR.
>
> En un escenario de mucha concentración de personas es normal que la señal de red funcione irregularmente, así que los lectores que se utilizan en cada acceso están pensados para poder operar incluso sin estar conectados al servidor.
>
> El lector decodifica el QR, y verifica el `mac`. Si la verificación es exitosa, revisa que `tipo_de_entrada` corresponda al lugar de ingreso, avisa al servidor por un canal seguro para confirmar el acceso y si el servidor da el _ok_ permite avanzar. El servidor verifica que `#ticket` corresponde a un ticket emitido, y que no haya sido utilizado. Si no puede hacerse la conexión o la respuesta demora demasiado, el lector permitirá el ingreso y registrará en su memoria `#ticket`, para rechazar usos subsiguientes del mismo ticket. Una vez retomada la conexión, enviará los números de ticket procesados al servidor.
>
> 1. Debido a la autenticación del `mac`, un atacante no puede inventar números de serie de tickets. Explicar qué tipo de fraude sí podría realizar abusando de fallas en el control de integridad y explicar cómo evitarlo. (1 pto).
> 2. Si un atacante consigue un lector y extrae la clave del evento, puede falsificar las entradas que quiera. Proponer un cambio para evitar que la extracción de claves del lector permita este tipo de ataque. (1 pto).
> 3. En un evento hay 4 entradas a diferentes secciones, un lector en cada una. Explicar cómo minimizar el problema de que dos personas utilicen la misma entrada en dos puertas distintas en un escenario de inestabilidad de la red. (1 pto).

**Conceptos:**

* [[Message Authentication Code]]
* [[Firma Digital]]
* [[Least Privilege]]
* [[Fail-Safe Defaults]]
* [[Principios de Diseño de Seguridad]]

1. `(event_id, tipo_de_entrada, #ticket, mac)` con `mac = mac_{k_evento}(event_id || #ticket)`. El `tipo_de_entrada` no está incluido en el MAC, así que un atacante puede comprar un ticket de _campo_ (el más barato) y cambiarle el campo `tipo_de_entrada` por _vip_: el MAC sigue siendo válido porque solo cubre `event_id || #ticket`, que no cambiaron.

   Esto es un problema de integridad, porque el MAC debe garantizar que ningún dato del ticket fue adulterado, y al no incluir `tipo_de_entrada` no protege ese campo. La solución es incluirlo dentro del MAC: `mac = mac_{k_evento}(event_id || tipo_de_entrada || #ticket)`.

2. Para evitar que cualquiera con la clave del evento pueda generar tickets válidos, se reemplaza el MAC simétrico por un esquema de **[[Firma Digital]]**: el servidor es el único que firma con su clave privada, y los lectores solo tienen la clave pública para verificar. Esto se alinea con el principio de **[[Least Privilege|menor privilegio]]**, ya que la única función de los lectores es verificar que los tickets no fueron falsificados — no necesitan poder generar tickets válidos.

3. La red inestable genera un trade-off entre disponibilidad (permitir el ingreso sin conexión) e integridad (evitar el doble uso de un ticket). Se proponen dos mecanismos complementarios:
   - Comunicación local entre lectores de una misma sección (LAN o Bluetooth) que no dependa del servidor: cuando un lector acepta un ticket offline, avisa a los demás lectores de su sección para que rechacen ese mismo ticket.
   - Como fallback si la comunicación local tampoco es viable: particionar los rangos de tickets por puerta dentro de cada sección, de forma que cada ticket solo sea válido en una puerta específica.

   El sistema prioriza la disponibilidad al permitir el ingreso sin conexión (**[[Fail-Safe Defaults|fallar de forma segura]]**), y estas medidas buscan recuperar la integridad sin sacrificar esa disponibilidad.
