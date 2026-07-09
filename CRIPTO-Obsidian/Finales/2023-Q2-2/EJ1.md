> [!example] Enunciado
> Una franquicia de bingos decide crear una aplicación que simule un bingo virtual donde los participantes paguen y obtengan dinero real. Cada juego permite obtener un cartón de 15 números elegidos al azar entre 1 y 90. El servidor va "sacando" números; cuando un participante completa su cartón presiona "bingo".
>
> Resuelta la seguridad de introducir/retirar dinero, se identificaron tres funcionalidades a resolver recurriendo solo a funciones criptográficas **simétricas**:
>
> a) El servidor debe generar una secuencia de números entre 1 y 90 que luego pueda ser reproducida por los clientes al finalizar la partida (para garantizar que no se favoreció a alguien en el sorteo).
> b) Los cartones se crean en las apps cliente al inicio de la partida; el servidor no los conoce hasta el momento de verificar un bingo, pero debe poder verificar que no fue modificado luego de empezado el juego.
> c) La app de cada participante debe poder verificar si el ganador realmente completó el bingo y emitir una alerta en caso contrario. Las apps no se comunican entre sí, solo a través del servidor.
>
> Nota: hay aproximadamente 2^55 cartones posibles (combinatoria de 90 tomados de a 15); asumir que un atacante podría probar esa cantidad de combinaciones.

**Conceptos:**

* [[Commitment Scheme]]
* [[Funciones de Hash criptograficas]]
* [[Message Authentication Code]]
* [[Nonce]]

a) Antes de empezar la partida, el servidor selecciona los números que va a ir sacando y se compromete a ellos por adelantado, cifrándolos con una clave simétrica (o publicando su hash), de forma análoga a un **[[Commitment Scheme]]**. Al finalizar, revela la clave (o los valores) para que los clientes puedan reproducir la secuencia exacta y confirmar que no fue alterada sobre la marcha para favorecer a nadie.

b) Que el servidor pueda verificar que el cartón no fue modificado luego de empezado el juego es un problema de **integridad**: cada app cliente debe generar su cartón y comprometerse a él (por ejemplo enviando al servidor un [[Message Authentication Code|MAC]] o hash del cartón, opcionalmente con un [[Nonce|nonce]] dado que el espacio de cartones — aunque grande, ~2^55 — podría eventualmente atacarse por fuerza bruta si no se agrega aleatoriedad). Al reclamar "bingo", la app revela el cartón completo y el servidor verifica que corresponda al compromiso enviado al inicio, exactamente igual que en un esquema de commitment: el jugador no puede "armar" un cartón ideal después de ver 15 números porque ya se comprometió antes de que empezara el sorteo.

c) Cada app, para poder verificar de forma independiente que el ganador realmente completó el bingo sin comunicarse directamente con las demás, necesita recibir del servidor tanto la secuencia de números sacados (verificable por el punto a) como el cartón revelado por el ganador (verificable por el punto b). Con esos dos datos, cada app puede recalcular localmente si los 15 números del cartón efectivamente salieron en la secuencia, y emitir una alerta si el servidor declaró ganador a alguien cuyo cartón no está completo — sin necesitar canal directo entre participantes, ya que toda la evidencia necesaria pasa por el servidor y es verificable criptográficamente por cualquiera.
