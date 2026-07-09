> [!example] Enunciado
> Una página de noticias tiene un usuario y un generador de noticias. Cada noticia puede tener etiquetas (categoría, ubicación). Cada usuario lleva un puntaje (+1/-1 por rating). La página tiene API REST por HTTPS. Se le paga al generador por visualizaciones.
>
> Se encontró que:
> 1. Existe una URL para pedir todas las noticias `https://dominio.com/news?limit=100&page=20`
> 2. Recomendar/no recomendar se hace vía `https://dominio.com/{generator_id}/{news_id}/upvoted` o `.../downvoted`
> 3. Las personas se registran por usuario y password
> 4. Cualquier tag HTML ingresado aparece como texto (input sanitizado)
>
> Establecer 4 hipótesis de falla con las respectivas pruebas que haría.

**Conceptos:**

* [[CWE-89 - SQL Injection]]
* [[Broken Access Control]]
* [[Denial of Service (DoS)]]
* [[Improper Authentication]]

1. **SQL Injection en los parámetros de búsqueda de noticias**

   **Hipótesis:** `?limit=100&page=20` probablemente se traduce en una query tipo `SELECT * FROM noticias LIMIT 100 OFFSET 2000`. Si no está parametrizada, se puede inyectar SQL para extraer datos que no deberían ser públicos.

   **Prueba:** `/news?limit=100&page=20 UNION SELECT email, password FROM usuarios; --`, o como prueba menos intrusiva `/news?limit=100&page=20' OR '1'='1`. Si devuelve más resultados o datos de otras tablas, se confirma.

2. **[[Broken Access Control]] — Manipulación de votos a través de IDs en la URL**

   **Hipótesis:** La URL de votación expone los IDs directamente. Si el servidor solo verifica autenticación, un atacante puede cambiar `news_id` para votar noticias no leídas, repetir el request para inflar el puntaje de un generador, o cambiar `upvoted` por `downvoted` para bajar a un competidor.

   **Prueba:** Loguearse e interceptar el request de upvote; cambiar `news_id` por otro, y reenviar el mismo upvote varias veces para ver si el servidor acepta votos duplicados.

3. **[[Denial of Service (DoS)|DoS]] por manipulación del parámetro `limit`**

   **Hipótesis:** Si el servidor no valida el valor de `limit`, un atacante puede forzar respuestas masivas que saturen BD, memoria y ancho de banda.

   **Prueba:** Enviar `limit=1000`, `limit=100000`, `limit=999999999` (sin abusar, 2-3 requests) y observar si el servidor responde sin rechazar ni limitar.

4. **[[Improper Authentication]] — Falta de política de contraseñas y rate limiting**

   **Hipótesis:** El sistema requiere usuario/contraseña sin mención de MFA ni límite de intentos; hay incentivo económico (pago por visualizaciones) para tomar cuentas ajenas.

   **Prueba:** Intentar registrarse con contraseñas débiles ("123456"); realizar 20 intentos de login incorrectos para verificar rate limiting.
