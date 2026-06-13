**PAM** (_Privileged Access Management_) es el conjunto de prácticas y herramientas para **proteger y gestionar** el uso de cuentas con acceso privilegiado a sistemas críticos.

## ¿Por qué son especialmente sensibles?

Las cuentas privilegiadas pueden:

- Otorgar capacidades amplias a los usuarios que las poseen.
- Modificar configuraciones de seguridad del sistema.
- Acceder a datos altamente confidenciales.
- Supervisar y modificar otras cuentas de usuario.

Son el objetivo prioritario del [[Malware Attack Cycle]] en la fase de **escalamiento de privilegios**.

## Prácticas y controles

| # | Práctica | Descripción |
|---|---|---|
| 1 | **Inventario** | Mantener un inventario completo de todas las cuentas privilegiadas para garantizar visibilidad y control. |
| 2 | **Least Privilege** | Los usuarios solo tienen el acceso mínimo necesario. Ver [[Least Privilege]]. |
| 3 | **MFA** | Autenticación multifactor obligatoria para cuentas privilegiadas, garantizando también el **no-repudio**. Ver [[Multi-Factor Authentication]]. |
| 4 | **Registro y monitoreo** | Todas las actividades realizadas con cuentas privilegiadas se registran y monitorean para detectar actividades sospechosas. |
| 5 | **Rotación de contraseñas** | Las contraseñas se cambian regularmente de forma automática para reducir el riesgo de compromiso prolongado. Ver [[Expiración de Claves]]. |

## Ciclo de vida (PAM Lifecycle)

```
Descubrimiento → Protección → Monitoreo → Análisis → Respuesta → Auditoría
```

## Relación con otros conceptos

- [[Least Privilege]]: principio base del PAM.
- [[Multi-Factor Authentication]]: control técnico requerido.
- [[RBAC]]: el modelo de roles sobre el que se implementa.
- [[ZTA - Componentes]]: el IAM de ZTA gestiona las identidades privilegiadas.
- [[Separation of Privilege]]: las cuentas privilegiadas deben estar segregadas por función.
- [[Gobernanza]]: el PAM es un componente del programa de gobernanza.
