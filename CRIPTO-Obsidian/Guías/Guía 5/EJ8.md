> [!note] Enunciado
> Se comparan los servicios que provee la firma digital y los MAC. Oscar puede observar los mensajes entre Alice y Bob, pero no conoce ninguna clave salvo las públicas.
>
> Determinar si el ataque se puede detectar con Firma Digital, MAC, ambos o ninguno.
>
> a) Alice envía $x =$ "Transferir \$1000 a Mark" en plano y también envía $sign(x)$ a Bob. Oscar intercepta y reemplaza "Mark" con "Oscar".
> b) Alice envía $x =$ "Transferir \$1000 a Oscar" con $sign(x)$. Oscar reenvía el mensaje 100 veces a Bob.
> c) Oscar afirma que él envió un mensaje $x$ con $sign(x)$ válida a Bob. Alice afirma que fue ella.
> d) Bob dice recibir $x =$ "Transferir \$1000 de Alice a Bob" con $sign(x)$ válida de Alice. Alice niega haberlo enviado.

## a) Modificación del mensaje (*Message Integrity*)

**Detectable con: Firma Digital y MAC.**

Tanto la firma digital como el MAC cubren el contenido del mensaje. Si Oscar modifica "Mark" por "Oscar", la firma o el MAC dejan de ser válidos y Bob puede detectar la manipulación.

## b) Reenvío del mensaje (*Replay*)

**Detectable con: ninguno.**

Oscar reenvía el mensaje original sin modificarlo, junto con la firma original válida. Ni la firma digital ni el MAC tienen mecanismos intrínsecos para detectar repeticiones. Se necesitaría un mecanismo adicional como timestamps o nonces.

## c) Disputa de autoría (*Cheating*)

**Detectable con: Firma Digital. NO con MAC.**

Con firma digital, la firma solo puede generarse con la clave privada de Alice, por lo que si se verifica con la clave pública de Alice, queda probado que fue Alice quien la generó. Oscar no puede haberla generado sin la clave privada de Alice.

Con MAC, tanto Alice como Oscar deberían revelar su clave secreta para probar quién generó el valor de hash, lo que no permite dirimirlo sin comprometer la clave.

## d) Repudio de un mensaje enviado (*Bob is cheating*)

**Detectable con: Firma Digital. NO con MAC.**

Con firma digital, Alice puede pedir a Bob que presente el mensaje y la firma. Si la firma se verifica con la clave pública de Bob... espera, la firma es de Alice. Si Alice puede mostrar que la firma se verifica con su propia clave pública y ella no la generó, hay una contradicción (implicaría que su clave privada fue comprometida). En la práctica, la firma digital provee **no repudio**: si la firma es válida bajo la clave pública de Alice, Alice no puede negar haberla enviado.

Con MAC, como la clave es compartida entre Alice y Bob, Bob podría haber generado el MAC él mismo, por lo que no hay forma de probar nada ante un tercero.
