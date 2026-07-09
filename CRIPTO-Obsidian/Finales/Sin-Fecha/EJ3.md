> [!example] Enunciado
> Honoris estima la reputación de un jugador integrando más de 100 juegos. Los administradores de cada juego definen hitos que otorgan puntuación; los administradores de Honoris definen un coeficiente por juego. Los desarrolladores notifican hitos vía una API REST que usa **TLS v1.0** y requiere un **API Secret** de 64 caracteres en los headers. Todavía no se contempla que la puntuación de los hitos varíe.
>
> Tras llegar a un millón de suscripciones, aparecieron muchísimas cuentas con reputación imposible de alcanzar en un año.
>
> 1. Establecer 4 hipótesis plausibles, ordenadas de más a menos probable.
> 2. Explicar cómo probaría cada hipótesis siguiendo la metodología de pentest.

**Conceptos:**

* [[TLS]]
* [[Sensitive Data Exposure]]
* [[Broken Access Control]]

**Hipótesis 1 (más probable): [[TLS]] v1.0 roto → Robo de API Secret → Notificación de hitos falsos**
- Por qué: TLS v1.0 tiene vulnerabilidades conocidas (BEAST, POODLE). Un atacante puede interceptar el tráfico e extraer el API Secret de los headers, y usarlo para notificar hitos falsos.
- Prueba: Capturar tráfico entre un juego y la API con un proxy e intentar descifrar el TLS v1.0 explotando sus vulnerabilidades conocidas; si se extrae el API Secret, usarlo para notificar un hito falso.

**Hipótesis 2: Falta de validación en la API — un juego notifica hitos de otro ([[Broken Access Control]])**
- Por qué: Si la API solo valida que el API Secret sea válido pero no que el hito corresponda al juego asociado, un juego con coeficiente bajo puede notificar hitos de un juego con coeficiente alto.
- Prueba: Con el API Secret de un juego de prueba, notificar un hito perteneciente a otro juego y ver si la API lo acepta.

**Hipótesis 3: Broken Access Control entre roles — admin de juego escala privilegios**
- Por qué: Si no se diferencian bien los roles de administrador de juego vs. de Honoris, un admin de juego podría modificar el coeficiente de su propio juego.
- Prueba: Con credenciales de admin de un juego, intentar `PUT /api/admin/juegos/{id}/coeficiente` y ver si el servidor lo acepta en vez de devolver 403.

**Hipótesis 4 (menos probable): API Secret en logs o código fuente público — [[Sensitive Data Exposure]]**
- Por qué: Con más de 100 juegos integrados, es probable que algún desarrollador haya expuesto su API Secret en un repositorio público o en el código del cliente.
- Prueba: Buscar en GitHub combinaciones tipo `"honoris" AND "api_secret"`, y revisar el código fuente del frontend de algunos juegos.
