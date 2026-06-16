Los componentes de la [[Zero Trust Architecture]] proveen la información y los mecanismos que el **Policy Engine** necesita para tomar decisiones de acceso en tiempo real.

## Componentes del plano de datos

### CDM — Continuous Diagnostics and Mitigation

El sistema CDM provee al motor de políticas información sobre el **estado del activo** que realiza una solicitud de acceso:

- ¿Tiene el antivirus actualizado?
- ¿Tiene los últimos parches de seguridad aplicados?
- ¿El dispositivo cumple con la postura de seguridad requerida?

### Logs de actividad + SIEM

- **Logs**: recopilan registros de activos, tráfico de red, acciones de acceso a recursos y otros eventos que proveen información en tiempo real sobre la postura de seguridad.
- **SIEM** (_Security Information and Event Management_): recolecta información de seguridad de múltiples fuentes para su análisis centralizado y correlación de eventos.

Ver: [[SIEM]]

### Threat Intelligence

Provee información de fuentes internas o externas que ayuda al motor de políticas a tomar decisiones de acceso. Puede incluir:

- Nuevos ataques o vulnerabilidades descubiertas.
- Indicadores de compromiso (IOCs).
- Feeds de [[Blacklist|blacklists]] de IPs y dominios maliciosos.

Recursos: https://github.com/hslatman/awesome-threat-intelligence  |  https://www.virustotal.com

## Componentes del plano de control

### IAM — Identity and Access Management

- Crea, almacena y gestiona las **cuentas de usuario** y los registros de identidad.
- Contiene información del sujeto (nombre, email, certificados) y características organizacionales (función, privilegios, derechos asignados).
- Implementación concreta de la [[Identidad del Sujeto]].

### PKI — Public Key Infrastructure

- Genera y registra los **certificados** emitidos por la empresa a recursos, sujetos, servicios y aplicaciones.
- Permite la autenticación criptográfica de identidades dentro de la arquitectura.

Ver: [[Criptosistema Asimétrico]], [[Firma Digital]]

### Políticas de acceso a datos

- Definen las características, reglas y condiciones de acceso a los recursos.
- Pueden ser codificadas estáticamente o generadas de forma **dinámica** por el Policy Engine.
- Constituyen el punto de partida para autorizar el acceso a un recurso.

### Industry Compliance

Garantiza que la empresa cumpla con los **regímenes normativos** aplicables:

| Normativa | Alcance |
|---|---|
| **HIPAA** | Protección de información de salud (EEUU) |
| **SOX** | Controles financieros y contables |
| **PCI-DSS** | Datos de tarjetas de crédito |
| **Banco Central** | Regulaciones financieras locales |

## Relación con otros conceptos

- [[Zero Trust Architecture]]: la arquitectura de la que son componentes.
- [[Open Policy Agent]]: herramienta para implementar el Policy Engine dinámico.
- [[RBAC]] / [[ABAC]]: modelos de control de acceso implementados por el IAM.
- [[Reglas basadas en Contexto]]: el CDM y Threat Intel proveen el contexto dinámico.
- [[Privileged Access Management]]: gestión especial de cuentas privilegiadas dentro del IAM.
