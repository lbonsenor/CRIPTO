> [!example] Enunciado
> Agrolin S.A. desarrolla **robo-cosecha**: dos drones, una cosechadora autónoma y un sistema de control integrado. Un dron mapea la zona a cosechar; la base programa el recorrido de la cosechadora; un segundo dron sobrevuela adelante para detectar anomalías y frenar la cosechadora si es necesario.
>
> La base se comunica punto a punto con los drones y la cosechadora por radiofrecuencia con un protocolo tipo UDP, con una capa segura similar a (d)TLS que da autenticación e integridad. El software de la base es Java; el de drones y cosechadora, C++.
>
> Establecer 4 hipótesis de vulnerabilidades con alta probabilidad de existir, describiendo hipótesis y prueba para cada una.

**Conceptos:**

* [[Sensitive Data Exposure]]
* [[CWE-787 - Buffer Overflow]]
* [[Fail-Safe Defaults]]
* [[Replay Attack]]

**Hipótesis 1: Falta de confidencialidad en la comunicación por radiofrecuencia**
- Hipótesis: La capa segura brinda "autenticación e integridad" pero no menciona confidencialidad. Sin cifrado, un atacante con un receptor de radiofrecuencia puede capturar imágenes del terreno, mapas, coordenadas y rutas — información comercialmente sensible del cliente.
- Prueba: Con un SDR cerca del campo durante una operación, capturar los paquetes y analizar si el contenido es legible en texto plano.

**Hipótesis 2: [[CWE-787 - Buffer Overflow|Buffer overflow]] en el software C++ de drones y cosechadora**
- Hipótesis: Si los parsers en C++ no validan correctamente el tamaño de los datos recibidos, un atacante podría enviar paquetes malformados que provoquen overflow, crasheando el dispositivo o permitiendo ejecución de código arbitrario.
- Prueba: Con un transmisor en la misma banda, enviar paquetes con campos de tamaño excesivo (coordenadas anómalas) y observar si los dispositivos crashean o se comportan erráticamente; o realizar fuzzing del parser en laboratorio.

**Hipótesis 3: Jamming / DoS sobre la señal de frenado del dron de seguridad**
- Hipótesis: La radiofrecuencia es susceptible a interferencia intencional. Si un atacante interfiere la señal de frenado, el punto crítico es qué hace la cosechadora sin comunicación: si sigue avanzando (fail-open) es un riesgo físico; si frena (**[[Fail-Safe Defaults|fail-safe]]**) es un vector de DoS.
- Prueba: Generar interferencia en la banda utilizada durante una operación y observar si la cosechadora frena automáticamente o sigue avanzando.

**Hipótesis 4: [[Replay Attack]] — reenviar comandos capturados**
- Hipótesis: Al ser datagramas (UDP), si no hay ventana de números de secuencia adecuada, un atacante puede capturar y reenviar comandos legítimos en otro momento (por ejemplo reenviar "zona limpia" cuando hay un obstáculo).
- Prueba: Capturar paquetes con un SDR durante una operación normal y reenviarlos después con un transmisor; verificar si se ejecutan.
