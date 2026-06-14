![[2-2025-Q1-2-EJ1.png]]
**Conceptos:**
- [[Modelo Clark-Wilson]]
- [[PostgreSQL - Policies]]
### a)
Bajo las condiciones planteadas (red única, sin firewalls ni restricciones de comunicación), la principal vulnerabilidad radica en **la exposición y el manejo de las credenciales de la base de datos**.

Si el tráfico no está fuertemente cifrado, cualquier atacante en la misma red local puede capturarlas fácilmente mediante técnicas de _sniffing_.

Al tener acceso directo a la red de la base de datos (por falta de firewalls) y poseer sus propias credenciales de base de datos, un usuario malintencionado o un atacante no necesita usar la interfaz del frontend ni pasar por las reglas del backend. Puede conectarse **directamente a la base de datos**, violando el modelo Clark-Wilson

### b)
**Propuesta 1: Pool de Conexiones**
La base de datos ya no debe conocer las credenciales individuales de cada empleado de la empresa. En su lugar, se configura un usuario genérico y dedicado exclusivamente para el backend (ej. `app_backend`). El backend maneja un _connection pool_ utilizando esta única credencial protegida.

**Cómo se mantiene la política:** La lógica de aislamiento (quién ve qué) se traslada o se expone a través del backend. Para no perder la ventaja de _Row Level Security (RLS)_ en la base de datos, el backend puede autenticarse con su usuario genérico y, al abrir la sesión de un empleado específico, pasar el ID del usuario como una **variable de sesión de la base de datos** (por ejemplo, usando `SET LOCAL app.current_user_id = 'X'`). RLS utilizará esa variable de sesión para filtrar las filas, manteniendo la restricción de que el empleado solo vea su sueldo.

**Propuesta 2: Firewalling**
Dividir la red única en zonas de seguridad aisladas (VLANs) y colocar firewalls intermedios.

La base de datos debe estar en una zona aislada (_Data Zone_) que **solo** acepte conexiones entrantes provenientes de la dirección IP específica del servidor de backend.
### c)

| Caracteristica                 | Arquitectura Original                                 | Propuesta                                              |
| ------------------------------ | ----------------------------------------------------- | ------------------------------------------------------ |
| **Autenticación en BD**        | Credenciales de cada usuario viajan/se usan en la BD. | Credencial única del Backend (oculta al usuario).      |
| **Flujo de Acceso**            | Directo si se saltea el backend (rompe Clark-Wilson). | Estrictamente intermediado por el Backend.             |
| **Aislamiento de Red**         | Nulo (Red única expuesta).                            | Alto (Segmentación por Firewalls).                     |
| **Punto de aplicación de RLS** | Depende de la sesión directa del usuario.             | Depende del contexto de sesión enviado por el Backend. |
Reduce el riesgo de un ataque siempre y cuando haya cifrado en tránsito