> [!example] Enunciado
> Una empresa tiene sensores que comunican a un dispositivo maestro sus niveles de sensado. Intercambian claves con Diffie-Hellman en la primera comunicación, y luego todos los mensajes (encriptados con AES-CBC) tienen la estructura:
>
> `#sensor || e_k(#sensor || valor_sensado || datos_adicionales)`
>
> 1. Un atacante logra que los mensajes se alteren y tengan datos adicionales inválidos, generando una señal de alerta falsa. ¿Cómo es posible? Solucionarlo sin nuevas claves.
> 2. Otro atacante envía repetidamente la señal de un sensor, haciendo que el maestro registre niveles incorrectos. ¿Cómo es posible? Modificar el mensaje para evitarlo sin nuevas claves.

**Conceptos:**

* [[Intercambio Diffie-Hellman]]
* [[CBC]]
* [[Bit-Flipping Attack]]
* [[GCM - Galois Counter Mode]]
* [[Replay Attack]]
* [[Chosen Ciphertext Attack]]

1. AES-CBC es CPA-secure pero no provee integridad: un atacante puede hacer un ataque de **[[Bit-Flipping Attack|bit-flipping]]** sobre el ciphertext. En [[CBC]], modificar bits del bloque cifrado `c_i` deja `m_i` como basura pero produce un bit-flip predecible en `m_{i+1}`; los bloques siguientes quedan intactos. El atacante puede modificar bits del bloque correspondiente a `valor_sensado` o `datos_adicionales` sin conocer la clave, generando una alerta falsa.

   **La solución:** el enunciado pide "sin nuevas claves", y agregar un MAC separado requeriría una clave independiente (cifrar y hacer MAC con la misma clave no es [[Chosen Ciphertext Attack|CCA-secure]]). La solución es cambiar a **[[GCM - Galois Counter Mode|AES-GCM]]**, que da cifrado autenticado con una sola clave (deriva internamente las claves de cifrado y autenticación). Con GCM, cualquier bit modificado invalida la autenticación y el mensaje se descarta en vez de generar alerta falsa.

2. El atacante captura un mensaje legítimo completo y lo reenvía repetidamente ([[Replay Attack]]). Como el mensaje es auténtico, incluso AES-GCM lo aceptaría. Se agrega un **counter**, tanto dentro como fuera del cifrado: `#sensor || counter || EncGCM_k(#sensor || counter || valor_sensado || datos_adicionales)`. El maestro guarda el último counter por sensor, rechaza si es igual o menor, y si es mayor verifica la autenticación de GCM y compara el counter interno con el externo. El atacante no puede modificar el counter externo sin invalidar la autenticación GCM.
