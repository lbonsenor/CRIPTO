![[EJ3.png]]

Siempre que $E_{k}$ sea biyectiva, es posible

En términos de error, si se corrompe un mensaje, se corrompen el resto de mensajes que quedan, idem en el caso de de-codificación

Esto es literalmente el mismo bloque de encripción [[CBC]], por lo tanto, es seguro con IVs aleatorios

En el caso de [[Counter (CTR)]], Counter no arrastra error, y tiene el mismo grado de confidencialidad que el caso anterior ([[Chosen Plain Text indistinguishability|CPA-Secure]] si k,nonce son aleatorios)

Lo mismo pasa con [[OFB]], aunque este no es paralelizable