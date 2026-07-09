> [!example] Enunciado
> Copérnico SRL posee varias aplicaciones web internas y está diseñando un sistema de login unificado (**SSO**), que permita a sus empleados autenticarse una vez y poder utilizar todas las aplicaciones a las cuales tenga acceso.
>
> Para eso ha decidido instalar un **Proveedor de Identidad (IdP)** que ofrece una aplicación web donde los usuarios pueden autenticarse. El IdP posee un par de claves `(sk, pk)` utilizadas para realizar firmas digitales. Todas las aplicaciones web conocen la clave pública `pk`.
>
> Cuando un usuario llega a cualquier aplicación, si no posee una sesión válida, la aplicación redirige al usuario al IdP, donde se autentica. Cuando el proceso es exitoso, el IdP construye un _token_ `T = (id_usuario, servicio)`, donde `servicio` es el identificador del servicio que solicitó el _login_.
>
> Luego calcula `S = Sign_{sk}(H(T))`, donde `Sign` es una firma digital resistente a falsificaciones y `H(.)` es una función de hash **libre de colisiones**. Finalmente, redirige al usuario a la página del servicio, enviando `T` y `S` como parámetros.
>
> Los distintos servicios, ante la presencia de los parámetros `T` y `S`, verifican la firma `S` y si es válida y el servicio mencionado en el token es el mismo que está validando la firma, crean una sesión para el usuario.
>
> 1. Explicar por qué cada servicio no necesita conocer la contraseña del usuario para poder crearle una sesión y eso no tiene un impacto negativo en la seguridad.
> 2. ¿Cómo se puede mejorar el protocolo para evitar el escenario donde un atacante consiga de algún modo un par `T,S` válido e impersone al usuario que los generó en primer momento?
> 3. Un desarrollador propone optimizar el sistema eliminando el campo "servicio" del token. Desarrollar un ataque que explote esta modificación para ganar acceso a un sistema w' robando un par de T,S que permiten acceso al sistema w. Asumir que las comunicaciones entre partes están correctamente protegidas por un canal TLS y explicar un posible escenario que le permite "robar" esos tokens.

**Conceptos:**

* [[SSO - Single Sign-On]]
* [[Firma Digital]]
* [[Funciones de Hash criptograficas]]
* [[Replay Attack]]
* [[Denning-Sacco]]
* [[Least Privilege]]
* [[TLS]]

1. Cada servicio no necesita conocer la contraseña del usuario porque la autenticación está centralizada en el IdP. El usuario se autentica únicamente ante el IdP, que genera un token T firmado con su clave privada sk. Los servicios solo verifican la firma ejecutando `Vrfy_pk(H(T), S)`: calculan H(T) por su cuenta y verifican que S sea una firma válida usando la clave pública pk del IdP. Si pasa, el servicio sabe que el token fue generado por el IdP (solo él posee sk) y que el usuario fue autenticado legítimamente.

   Esto no daña la seguridad, la mejora: en un modelo sin SSO cada servicio tendría que gestionar contraseñas, multiplicando la superficie de ataque. Al centralizar la autenticación en el IdP, esa superficie se reduce a un único punto, y los servicios solo poseen pk (información pública). Se alinea con **[[Least Privilege|menor privilegio]]**: cada servicio tiene solo la información mínima necesaria (pk) para cumplir su función.

2. El ataque que se busca prevenir es un **[[Replay Attack]]**: un atacante que consigue un par (T, S) válido puede reenviarlo tal cual para hacerse pasar por el usuario legítimo. Se agrega un timestamp al token: `T = (id_usuario, servicio, timestamp)`, firmado igual que antes. El servicio verifica además que el timestamp esté dentro de una ventana aceptable, rechazando tokens viejos. El atacante no puede modificar el timestamp para extender la validez porque invalidaría la firma S, y generar una firma nueva requiere sk. Esto es análogo a la mejora de **[[Denning-Sacco]]** sobre [[Protocolo Needham-Schroeder|Needham-Schroeder]].

3. Sin el campo servicio, `T = (id_usuario)` es válido para **cualquier servicio**, ya que los servicios solo verifican que la firma sea válida para ese id_usuario. **El ataque:** un usuario se autentica legítimamente en el servicio w. El administrador malicioso de w tiene ahora ese par (T, S) — al no contener campo servicio, puede presentarlo ante el servicio w' y ganar acceso como si fuera el usuario original.

   **Escenario de robo con TLS:** TLS protege los datos en tránsito, pero una vez que (T, S) llega al servicio w, queda almacenado ahí (logs, memoria, BD de sesiones). El administrador de w tiene acceso completo a su propio servidor y puede extraerlo de ahí — TLS no protege contra esto porque el dato ya llegó a destino. Por eso el campo servicio es esencial: con `T = (id_usuario, servicio)`, un token generado para w no sería válido para w'.
