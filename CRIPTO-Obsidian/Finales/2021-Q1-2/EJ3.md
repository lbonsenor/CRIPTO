> [!example] Enunciado
> Agrolin S.A. construye soluciones tecnológicas para el campo. Un producto permite seguimiento de rendimiento de cosechas y sugiere rotaciones óptimas. El sistema es un frontend web (SPA) donde el usuario se registra, autentica, da de alta lotes en un mapa, y compara rinde estimado vs obtenido. La app llama a un backend vía API REST.
>
> Establecer 4 hipótesis de vulnerabilidades con alta probabilidad de existir, describiendo únicamente la hipótesis y la prueba para confirmarla o refutarla.

**Conceptos:**

* [[Improper Authentication]]
* [[CWE-89 - SQL Injection]]
* [[Denial of Service (DoS)]]
* [[Improper Authorization]]

**Hipótesis 1 — [[Improper Authentication]]**
- Hipótesis: El sistema puede tener autenticación débil o inexistente que permita loguearse a cuentas ajenas o registrarse con credenciales muy simples y realizar intentos ilimitados.
- Prueba: Intentar entrar al sitio para ver si hay autenticación; registrarse con credenciales simples ("usuario"/"contraseña") y, si son aceptadas, probar un ataque de diccionario con contraseñas comunes.

**Hipótesis 2 — [[CWE-89 - SQL Injection|SQL Injection]]**
- Hipótesis: Al ingresar datos del campo (delimitación, nombre) es posible insertar sentencias SQL por falta de validación.
- Prueba: Tras crear una cuenta, insertar en el nombre del campo algo como `nombre; CREATE TABLE prueba(int hola);--` y verificar si se ejecuta sin daño a la base real.

**Hipótesis 3 — [[Denial of Service (DoS)|DoS]]**
- Hipótesis: Mostrar el rinde esperado según datos históricos de todos los clientes de la zona y sugerir el cultivo más rentable implica varias queries costosas; muchas consultas en paralelo podrían saturar el servidor.
- Prueba: No ejecutar el ataque real; verificar únicamente si es posible realizar 2 consultas simultáneas desde la misma IP (con o sin autenticación) e informar la situación.

**Hipótesis 4 — [[Improper Authorization|Improper Authorization]]**
- Hipótesis: Una vez logueado, el sistema podría no manejar bien los permisos, permitiendo acceder a recursos de otro cliente (por ejemplo el campo de otro usuario) cambiando un ID en la URL.
- Prueba: Con una cuenta propia, ver la URL de un campo propio (`.../cliente/12/campo/1`) y probar acceder a `.../cliente/1/campo/1` sin modificar nada, solo confirmando el acceso indebido.
