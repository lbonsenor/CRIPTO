# Respuesta ante Incidentes

La **respuesta ante incidentes** (_incident response_) es el proceso estructurado para **detectar, contener, erradicar y recuperarse** de un incidente de seguridad que afecte la confidencialidad, integridad o disponibilidad de los datos.

## Fases del proceso

```
Preparación → Detección → Contención → Erradicación → Recuperación → Lecciones aprendidas
```

| Fase | Descripción |
|---|---|
| **Preparación** | Definir el equipo, los procedimientos, las herramientas y los contactos antes de que ocurra un incidente. |
| **Detección** | Identificar que ocurrió un incidente. Fuentes: [[DLP - Data Loss Prevention]], SIEM, alertas. |
| **Contención** | Limitar el daño: aislar sistemas afectados, bloquear accesos comprometidos. |
| **Erradicación** | Eliminar la causa raíz: remover malware, parchear vulnerabilidades, revocar credenciales comprometidas. |
| **Recuperación** | Restaurar los sistemas al estado operativo normal desde backups verificados. |
| **Lecciones aprendidas** | Analizar el incidente y mejorar los controles para evitar repetición. Actualizar [[Threat Modeling]]. |

## Obligaciones regulatorias

Varias regulaciones imponen plazos de notificación ante brechas:

| Regulación | Plazo de notificación |
|---|---|
| [[GDPR]] | 72 horas a la autoridad supervisora |
| [[HIPAA - PCI-DSS - SOX\|HIPAA]] | 60 días a los afectados |
| [[Ley 25.326]] | A la AAIP sin demora |

## Evidencia y forense

La respuesta ante incidentes requiere preservar **evidencia forense** sin contaminarla:

- Copias de memoria y discos antes de contener.
- Logs del sistema ([[Retención de Información]]).
- Cadena de custodia de la evidencia.

## Relación con otros conceptos

- [[Data Governance]]: la respuesta es la fase _Respond_ del ciclo de gobernanza.
- [[DLP - Data Loss Prevention]]: la detección del incidente puede originarse en una alerta DLP.
- [[Malware Attack Cycle]]: el análisis forense reconstruye las fases del ciclo de ataque.
- [[Defense in Depth]]: los incidentes prueban la efectividad de las capas de defensa.
- [[Retención de Información]]: los logs retenidos son la evidencia para el análisis forense.

## Referencia

https://comodosslstore.com/blog/data-breach-incident-response-complete-checklist.html
