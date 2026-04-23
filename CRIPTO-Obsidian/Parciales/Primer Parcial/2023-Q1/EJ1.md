![[Parciales/Primer Parcial/2023-Q1/Images/EJ1.png]]

a)
Es un **protocolo de establecimiento de claves y autenticación** (Key Exchange and Authentication Protocol).

El objetivo principal es construir un **canal seguro** entre el Cliente (C) y el Servidor (S). Para lograrlo, el protocolo busca:
- **Autenticación del Servidor:** A través del envío del certificado en el paso (1.2).
- **Acuerdo de Claves (Key Agreement):** Establecer una clave de sesión compartida (KCS​) que sea fresca (única para esta sesión) y secreta, utilizando tanto valores aleatorios (nonces NC​,NS​,N) como un secreto pre-establecido o transmitido (kS).
- **Confidencialidad e Integridad:** Preparar el terreno para que los datos en (1.6) y (1.7) viajen cifrados.

b)
Estos se conocen como mensajes de **"Finished"** o confirmación del handshake. Sus propósitos específicos son:
1. **Verificación de la Clave:** Confirman que ambas partes han derivado correctamente las mismas claves de sesión (KCS​ y k1). Si una de las partes hubiera calculado mal la clave, el descifrado o la verificación del MAC fallarían.
2. **Integridad del Handshake:** Al incluir un MAC (Message Authentication Code) de un `timestamp` (o a veces de un resumen de los mensajes anteriores), aseguran que los pasos previos no fueron alterados por un atacante en el medio.
3. **Prueba de Posesión (Liveness):** El uso de cifrado con la nueva clave KCS​ y el MAC con k1 demuestra que la contraparte está "viva" y posee el material criptográfico necesario en ese preciso instante.

c)
Es una buena práctica usar claves distintas para propósitos distintos. K0​ se usa para el intercambio/protección del secreto inicial, mientras que KCS​ se usa para el cifrado de datos. Esto evita que una debilidad en un algoritmo (ej. el de cifrado de datos) comprometa la clave maestra.