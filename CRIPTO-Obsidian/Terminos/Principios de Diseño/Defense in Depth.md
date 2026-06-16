> Principio 7 de [[Principios de Diseño de Seguridad]]

**Mecanismos exclusivos** (defensa en profundidad) establece que distintos mecanismos de seguridad **no deben compartir recursos, algoritmos ni hardware**, y que deben existir controles compensatorios para cuando uno falle o sea superado.

## Concepto

No basta con un único control de seguridad. Se deben diseñar **múltiples capas independientes** de controles, de modo que si una capa es comprometida, las siguientes limiten el daño.

## Ejemplo

> Se aplican [[Separation of Privilege|segregación de tareas]] y [[Least Privilege|mínimos privilegios]], pero una vulnerabilidad en el sistema permite a un atacante llevar adelante una venta sin solicitar las autorizaciones correspondientes.

¿Qué controles compensatorios se agregan?
- Alertas que detecten transacciones que omitieron el flujo de aprobación.
- Bloqueo automático de la transacción ante anomalías.
- Confirmación posterior por un segundo canal.

## Capas típicas de defensa

| Capa | Ejemplo |
|---|---|
| Perímetro de red | Firewall, IDS/IPS |
| Autenticación | MFA, [[TOTP]] |
| Autorización | [[RBAC]], [[AWS IAM]] |
| Datos | Cifrado en reposo y en tránsito |
| Monitoreo | Logs, alertas, SIEM |

## Relación con otros conceptos

- [[Separation of Privilege]]: una capa de la defensa en profundidad.
- [[Least Privilege]]: otra capa complementaria.
- [[Fail-Safe Defaults]]: cada capa debe fallar de forma segura.
- [[Threat Modeling]]: identifica qué capas son necesarias en un sistema específico.
- [[Modelo Bell-La Padula]]: modelo de seguridad multinivel que aplica este principio.
