![[EJ1.png]]
a) Tipo de protocolo y objetivo

Este **no es un protocolo de intercambio de claves**, porque:

- No genera una nueva clave de sesión
- Ya existen claves compartidas $K$ y $K'$

Es un **Protocolo de autenticación mutua con desafío–respuesta (challenge-response)**, con el objetivo de:
- Autenticar a ambas partes (A y B)
- Garantizar **frescura** (uso de nonces evita replay)
- Verificar **posesión de claves compartidas**

