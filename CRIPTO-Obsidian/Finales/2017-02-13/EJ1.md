> [!example] Enunciado
> Tecnosalud S.A. desarrolla un chip subdérmico con la historia clínica de una persona, con información separada en: inmutable (nombre, fecha de nacimiento, grupo sanguíneo), de contacto (modificable), y médica (solo se puede agregar). La información debe estar cifrada, ilegible sin dispositivo autorizado, y un dispositivo robado debe poder inutilizarse. La información nunca viaja a servidores centralizados.
>
> Cada chip y lector tienen número de serie y clave simétrica asociada (única por chip). Un comando central (CC) tiene la BD de números de serie y claves. Para acceder, el dispositivo (DS) consigue el número de serie del chip (CH) por un protocolo tipo RFID y solicita permiso al CC, que entrega un token (capacidad).
>
> 1. Describir un posible protocolo de mensajes entre CH, DS y CC para que el DS acceda a información del CH.
> 2. Explicar cómo un dispositivo robado no sigue accediendo indefinidamente. ¿Se puede evitar que vuelva a leer chips para los que ya tenía permiso?
> 3. Explicar por qué no es posible que alguien simule ser un dispositivo y acceda al chip.
> 4. Explicar por qué el protocolo no puede ser offline. ¿Qué requerimiento no se cumple sin un comando central?

**Conceptos:**

* [[Denning-Sacco]]
* [[Key Distribution Centers]]
* [[Blacklist]]

1. `CH → DS`: `#serie_chip` (broadcast tipo RFID). `DS → CC`: `#serie_DS || #serie_CH || permisos_solicitados || r1`; el CC verifica que el DS esté registrado y no inutilizado, que el chip exista, que los permisos sean compatibles con el tipo de dispositivo, y genera una clave de sesión `k_s`. `CC → DS`: `#serie_DS || r1 || k_s || permisos_otorgados || {#serie_DS || k_s || timestamp || permisos_otorgados}_k_CH`, todo cifrado con `k_DS`. `DS → CH`: reenvía el token cifrado con `k_CH`; el chip lo descifra (solo él y el CC conocen `k_CH`), verifica el timestamp (anti-replay) y los permisos, y obtiene `k_s`. `CH → DS`: `{datos}_k_s` — la información nunca pasa por el CC.

2. **Para tokens ya otorgados:** el timestamp incluido en el token limita su validez a una ventana corta; pasado ese tiempo, el chip lo rechaza. **Para tokens nuevos:** el CC marca el `#serie_DS` robado en una [[Blacklist]] y rechaza nuevas solicitudes de ese dispositivo. La limitación es que, como el chip es offline, un token ya emitido sigue siendo válido hasta que expire — por eso conviene que la ventana del timestamp sea corta (mecanismo de tipo **[[Denning-Sacco]]**).

3. Si el atacante inventa un `#serie_DS` no registrado, el CC rechaza la solicitud directamente. Si usa un `#serie_DS` legítimo (por ejemplo, visto impreso en algún lado), el CC cifra la respuesta con `k_DS`, que el atacante no conoce, por lo que no puede descifrarla ni obtener el token para presentar al chip.

4. Sin un [[Key Distribution Centers|comando central]] no se puede: (1) verificar qué dispositivos están autorizados y con qué permisos (el chip no tiene capacidad de almacenamiento ni actualización para eso), y (2) **inutilizar dispositivos robados** (blacklist) — el chip es offline por naturaleza y no puede consultar una revocación en tiempo real, así que un dispositivo robado seguiría accediendo indefinidamente si nadie centraliza esa decisión.
