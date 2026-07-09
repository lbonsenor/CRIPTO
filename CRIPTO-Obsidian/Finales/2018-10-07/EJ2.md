> [!example] Enunciado
> **Private Information Retrieval (PIR):** una parte consulta un registro de una base de datos sin que el servidor sepa cuál fue la consulta. La única implementación con garantías absolutas es enviar toda la BD y que el cliente elija localmente (poco práctico). Una implementación con restricciones usa **N servidores independientes**, donde cada entrada se cifra con un criptosistema de umbral (k, N); cada servidor guarda una sombra. Para recuperar una entrada, el cliente elige k servidores al azar y reconstruye localmente.
>
> 1. ¿Por qué desde un único servidor no es posible saber qué información fue solicitada?
> 2. ¿Cuáles son los valores mínimos de k y N para que el compromiso de 2 servidores no afecte la privacidad y la caída de hasta 4 no afecte la disponibilidad?
> 3. En la práctica, un servidor comprometido podría hacer él mismo los pedidos de las sombras necesarias. ¿Qué mecanismos implementaría para evitarlo?

**Conceptos:**

* [[Private Information Retrieval (PIR)]]
* [[Método de Shamir]]
* [[Firma Digital]]

1. Cada servidor tiene una sombra de cada entrada que por sí sola no tiene sentido ([[Método de Shamir|secreto compartido]]). El servidor no sabe qué dato está proporcionando cuando entrega una sombra. Además, los servidores son **independientes** (no se comunican entre sí): si `k` o más se pusieran de acuerdo, podrían juntar sus sombras y reconstruir el dato, por eso la seguridad depende de que no haya colusión.

2. `k > 2` (para que el compromiso de 2 servidores no alcance para reconstruir) y `n - 4 ≥ k` (para que la caída de hasta 4 no afecte disponibilidad), es decir `n ≥ k + 4`. Con `k = 3` → `n = 7`.

3. Se necesita distinguir un cliente legítimo de un servidor comprometido haciendo pedidos por su cuenta: los clientes firman sus pedidos con [[Firma Digital]] (autorización), y además las respuestas se cifran con la clave pública del cliente para que ni siquiera un servidor que intercepte la respuesta de otro pueda leerla: `Cliente → Servidor_i: pedido || pk_cliente || Sign_sk_cliente(pedido)`; el servidor verifica la firma, busca la sombra, y responde `Enc_{pk_cliente}(sombra)`. Un servidor comprometido que intercepte esa respuesta no puede descifrarla sin la sk del cliente, ni puede hacerse pasar por cliente sin una sk de cliente válida.
