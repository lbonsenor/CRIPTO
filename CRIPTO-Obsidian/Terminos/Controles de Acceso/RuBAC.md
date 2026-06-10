**Rule-Based Access Control (RuBAC)** es una variante del [[Control por Comprensión]] que se basa en una serie de **reglas de acceso explícitas**.

## Estructura básica de una regla

Cada regla contiene como mínimo:

| Campo | Descripción |
|---|---|
| **Principal** | El sujeto al que aplica la regla |
| **Objeto** | El recurso sobre el que aplica |
| **Tipo de Acceso** | La acción involucrada (leer, escribir, ejecutar...) |
| **Decisión** | Permitido o denegado |

## Resolución de múltiples reglas

Puede haber muchas reglas que apliquen al mismo acceso. Dependiendo de la implementación, se usa alguna política de [[Resolución de Reglas]]:

- [[First Match]]
- [[Best Match]]
- [[Deny over Allow]]

## Ejemplos de sistemas RuBAC

- [[SELinux]]: reglas definidas por contexto de proceso y recurso.
- [[Squid Web Proxy]]: reglas para filtrar dominios, protocolos y URLs.
- [[AWS IAM]]: policies con reglas Allow/Deny sobre acciones y recursos.
- [[API Gateway]]: reglas basadas en URL, headers, método HTTP.

## Relación con otros modelos

- [[RBAC]]: más simple, permisos por rol sin reglas granulares.
- [[ABAC]]: incorpora atributos del contexto, más flexible.
- [[Mandatory Access Control]]: puede implementarse como un conjunto de RuBAC aplicado obligatoriamente.
