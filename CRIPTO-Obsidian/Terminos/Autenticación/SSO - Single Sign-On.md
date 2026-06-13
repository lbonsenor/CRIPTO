**SSO** (_Single Sign-On_, autenticación remota) consiste en la **delegación de la autenticación** en un sistema externo, permitiendo al usuario autenticarse una vez y acceder a múltiples servicios.

## Características

- Implica una **relación de confianza** entre el servicio y el sistema de autenticación externo (_Identity Provider_, IdP).
- El servicio no gestiona las credenciales directamente: las delega al IdP.
- Reduce la cantidad de contraseñas que el usuario debe manejar.

## Productos y estándares

### Productos propietarios

| Producto | Descripción |
|---|---|
| **Active Directory Federation Services** (ADFS) | SSO de Microsoft, integrado con Windows |
| **CAS** (_Central Authentication Service_) | SSO open source para entornos web universitarios |
| **SiteMinder** | Producto SSO de Broadcom (ex CA Technologies) |

### Tecnologías abiertas

| Tecnología | Estado |
|---|---|
| **OpenID 1.0 / 2.0** | ❌ Obsoletos |
| **[[OpenID Connect]]** | ✅ Vigente — basado en [[OAuth 2.0]] |

## Relación con otros conceptos

- [[Autenticación]]: el proceso que SSO delega.
- [[OpenID Connect]]: el protocolo de SSO moderno y recomendado.
- [[OAuth 2.0]]: el estándar sobre el que se construye OpenID Connect.
- [[Multi-Factor Authentication]]: puede requerirse en el IdP como parte del flujo SSO.
- [[Identidad del Sujeto]]: SSO centraliza la gestión de identidades.
