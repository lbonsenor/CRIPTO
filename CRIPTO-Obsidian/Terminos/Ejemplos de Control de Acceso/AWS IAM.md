**AWS Identity and Access Management (IAM)** es el sistema de control de acceso de Amazon Web Services. Combina elementos de [[Mandatory Access Control]] y [[Discretionary Access Control]] en un mismo modelo.

## Tipos de políticas

| Tipo | Asociado a | Modelo |
|---|---|---|
| **Policies del Principal** | Usuario, rol o grupo IAM | MAC |
| **Policies del Recurso** | El recurso mismo (S3 bucket, Lambda, etc.) | DAC |

- Las **policies asociadas al Principal** definen qué puede hacer el sujeto (MAC).
- Las **policies asociadas al Recurso** definen quién puede acceder al recurso (DAC).

## Estructura de una policy

Las policies de IAM se escriben en JSON y definen:

- **Effect**: `Allow` o `Deny`.
- **Action**: la acción sobre el servicio (ej: `s3:GetObject`).
- **Resource**: el ARN del recurso afectado.
- **Condition**: condiciones adicionales (hora, IP, MFA, etc.) → [[Reglas basadas en Contexto]].

## Resolución de permisos

AWS aplica el principio de **[[Deny over Allow]]**: cualquier `Deny` explícito tiene precedencia sobre cualquier `Allow`.

## Relación con otros conceptos

- [[Mandatory Access Control]]: policies del Principal.
- [[Discretionary Access Control]]: policies del Recurso.
- [[RBAC]]: los roles IAM implementan control basado en roles.
- [[RuBAC]]: las policies son reglas explícitas de Allow/Deny.
- [[Deny over Allow]]: política de resolución en AWS.

## Referencia

https://www.bdrsuite.com/blog/aws-for-beginners-aws-managed-policies-and-inline-policies-part-10/
