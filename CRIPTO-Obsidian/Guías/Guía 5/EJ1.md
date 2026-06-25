> [!note] Enunciado
> Escribe un ejemplo de los siguientes ataques que pueden darse contra un protocolo. ¿Pueden evitarse?
> 1. Replay
> 2. Key Reuse
> 3. Man in the middle
> 4. Masquerading (suplantación de identidades)

## 1. Replay

El intruso repite o dilata información maliciosa o fraudulenta.

**Ejemplo:** Alice envía a Bob un cheque digital firmado por ella para que él cobre $100. Bob (tramposo) lo reenvía al banco más de una vez. El banco siempre certifica que está autorizado, y Bob le saca toda la plata a Alice.

**Solución:** Timestamps.

---

## 2. Key Reuse

Como las claves de sesión se generan en forma pseudoaleatoria, es posible predecir las claves de sesión y reusarlas.

**Ejemplo (Needham-Schroeder):** Si Mallory obtiene una clave de sesión anterior $K$ y viene guardando los mensajes de Alice a Bob, puede enviarle a Bob un mensaje viejo $E_B(K, A)$. Bob extrae $K$ y verifica que es de "Alice". Luego genera un número aleatorio $R_B$ y envía $E_K(R_B)$ a Alice. Mallory intercepta este mensaje y (en lugar de Alice) envía a Bob $E_K(R_B - 1)$. Bob se convence de que se está comunicando con Alice, pero en realidad lo hace con Mallory.

**Solución:** Timestamps y buenos métodos de generación de claves aleatorias.

---

## 3. Man in the Middle

El intruso actúa sobre los **dos canales** de comunicación (hacia A y hacia B). En general es también un *masquerading*.

**Ejemplo (Diffie-Hellman):**

- Alice envía el generador $g$, el primo $p$ y $g^a \mod p$.
- Mallory intercepta el mensaje de Alice. No puede hallar $a$, pero genera $g^m \mod p$ y se lo envía a Bob. Bob cree que es Alice, sigue el protocolo enviando $B = g^b \mod p$ y calcula $K = (g^m)^b \mod p$ como clave de sesión.
- Mallory hace lo mismo con Alice, quedando con una clave con Alice y otra con Bob. Alice encripta sus mensajes creyendo hablar con Bob, pero en realidad solo habla con Mallory, y viceversa.

---

## 4. Masquerading

Una persona o programa se hace pasar por otra falsificando datos (ej. Diffie-Hellman, Spoofing, suplantación de identidad).

Un *man in the middle* es habitualmente un *masquerading*, pero un *masquerading* no necesariamente es un *man in the middle*.
