Problema criptográfico donde una parte consulta información de un servidor (por ejemplo, un registro de una base de datos) sin que el servidor pueda saber cuál fue la información consultada.

La única construcción con garantías absolutas es que el servidor envíe la base de datos completa en cada consulta y el cliente elija localmente el registro de interés — impracticable en general.

Una implementación con restricciones más razonables usa **N servidores independientes**, donde cada entrada de la base de datos se protege con un [[Método de Shamir|esquema de secreto compartido]] de umbral `(k, N)`: cada servidor guarda una sombra de cada entrada. Para recuperar un dato, el cliente elige `k` servidores al azar, les pide la sombra de la entrada que le interesa, y reconstruye el secreto localmente.

Puntos clave:
- Ningún servidor individual puede saber qué se consultó, porque su sombra no tiene sentido por sí sola.
- La privacidad depende de que no haya colusión entre `k` o más servidores (deben ser independientes).
- Para que la caída de hasta `f` servidores no afecte la disponibilidad y el compromiso de hasta `c` servidores no afecte la privacidad, se necesita `N ≥ k + f` y `k > c`.
- En la práctica, para evitar que un servidor comprometido haga por su cuenta todos los pedidos necesarios para reconstruir el secreto, los pedidos del cliente deben ir firmados con [[Firma Digital]] (para autorización) y las respuestas cifradas con la clave pública del cliente (para que ni siquiera un servidor que intercepta la respuesta de otro pueda leerla).
