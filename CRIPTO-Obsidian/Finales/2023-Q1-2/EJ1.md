> [!example] Enunciado
> Hay una empresa que se encarga de controlar el agua, mediante dispositivos que informan mediciones a una central. Al instalar un dispositivo: emite su ID por broadcast, la central responde y le envía una llave **por HTTP**. Cuando el dispositivo envía lo recolectado, usa **AES-ECB**.
>
> Una auditoría reporta fallas en la comunicación de la llave y problemas de integridad en la comunicación de los dispositivos.
>
> 1. ¿Usando HTTPS se hubieran solucionado todos los problemas informados por la auditoría?
> 2. En caso de no poder usar HTTPS por presupuesto, ¿qué otro protocolo utilizaría para el intercambio de claves?
> 3. ¿Qué cambios son necesarios para asegurar la integridad en el envío de información de los dispositivos hacia la central?

**Conceptos:**

* [[Ataque Man-In-The-Middle]]
* [[GCM - Galois Counter Mode]]
* [[Message Authentication Code]]
* [[Key Distribution Centers]]
* [[Chosen Ciphertext Attack]]

1. HTTPS soluciona el problema de la comunicación de la clave: cifra el canal entre la central y el dispositivo, evitando que un atacante intercepte la key en tránsito ([[Ataque Man-In-The-Middle|MITM]]). Pero **no soluciona** el problema de integridad en la comunicación dispositivo → empresa, porque esa comunicación usa AES-ECB, que solo cifra (sin MAC ni autenticación, un atacante puede modificar bloques cifrados sin ser detectado) y además no es CPA-secure (bloques iguales de texto plano producen bloques iguales de cifrado).

2. Usar un protocolo de intercambio de claves con la central como [[Key Distribution Centers|KDC]]. Si los dispositivos solo tienen capacidad simétrica (más probable para sensores), cada uno viene de fábrica con una clave pre-compartida `k_i`; la central la usa para enviar la clave de sesión: `Enc_{k_i}(key_sesion || timestamp || MAC_{k_i}(key_sesion || timestamp))`. El dispositivo descifra, verifica el MAC y el timestamp, y usa `key_sesion` para comunicarse con la empresa.

3. Reemplazar AES-ECB por **[[GCM - Galois Counter Mode|AES-GCM]]** (o AES-CCM), que provee cifrado autenticado en una sola operación. Si no se puede cambiar el algoritmo, la alternativa es agregar un [[Message Authentication Code|MAC]] independiente con una clave distinta a la de cifrado (usar la misma clave para ambos no es [[Chosen Ciphertext Attack|CCA-secure]]): `AES-ECB_{key_i}(datos) || HMAC_{key_mac}(datos)`.
