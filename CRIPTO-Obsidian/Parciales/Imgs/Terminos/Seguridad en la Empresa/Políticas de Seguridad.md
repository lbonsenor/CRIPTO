Las **políticas de seguridad** son el marco normativo interno que define cómo la organización protege sus activos de información. Son un componente de la [[Gobernanza]].

## Función

- Traducen los objetivos de seguridad del negocio en **reglas concretas** de comportamiento y operación.
- Definen **responsabilidades** de cada rol ante los activos de información.
- Sirven de base para los controles técnicos ([[Control de Acceso]], [[Autenticación]], [[Mandatory Access Control]]).
- Son el punto de referencia para **auditorías** y evaluaciones de cumplimiento.

## Tipos de políticas comunes

| Política | Descripción |
|---|---|
| Política de contraseñas | Complejidad, expiración, reutilización. Ver [[Claves]]. |
| Política de control de acceso | Quién puede acceder a qué recursos. Ver [[Discretionary Access Control]], [[RBAC]]. |
| Política de uso aceptable | Qué pueden y no pueden hacer los usuarios con los recursos. |
| Política de clasificación de datos | Niveles de sensibilidad de la información. Ver [[Modelo Bell-La Padula]]. |
| Política de respuesta a incidentes | Procedimientos ante un incidente de seguridad. |
| Política de gestión de parches | Frecuencia y proceso de actualización de software. |

## Buenas prácticas de diseño

- Deben estar **alineadas con los principios de seguridad**. Ver [[Principios de Diseño de Seguridad]].
- Deben ser **simples de entender y cumplir** — [[Psychological Acceptability]].
- Deben revisarse periódicamente en el marco del [[Risk Management]].

## Relación con otros conceptos

- [[Gobernanza]]: las políticas son su expresión normativa concreta.
- [[Risk Management]]: las políticas son la respuesta a los riesgos identificados.
- [[Principios de Diseño de Seguridad]]: base conceptual de las políticas.
- [[Modelo de Seguridad]]: los modelos formales (Bell-La Padula, Biba, Clark-Wilson) guían el diseño de políticas.
