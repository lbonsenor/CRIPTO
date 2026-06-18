La **retención de información** es el guardado de datos que ya no son necesarios para el negocio, pero que deben conservarse por razones **regulatorias, legales o de análisis forense**.

## Características

- Los datos retenidos no están activos en el sistema operacional — se archivan en almacenamiento de bajo costo.
- La retención tiene **plazo definido**: cada regulación o ley establece el tiempo mínimo (y a veces máximo) de retención.
- Al final del período de retención, los datos deben **eliminarse de forma segura** (secure deletion).

## Fines de la retención

| Fin | Descripción |
|---|---|
| **Legal** | Cumplir con requisitos de regulaciones (GDPR, HIPAA, SOX, Ley 25.326). |
| **Forense** | Preservar evidencia para investigaciones de incidentes de seguridad. |
| **Auditoría** | Permitir verificar el comportamiento del sistema en un momento pasado. |

## Tensión con el GDPR

El [[GDPR]] establece el principio de **storage limitation**: los datos no deben retenerse más tiempo del necesario. Esto puede entrar en tensión con otras regulaciones que exigen períodos mínimos de retención. Las organizaciones deben balancear ambos requisitos con políticas claras.

## Relación con otros conceptos

- [[Data Life Cycle]]: la retención es la etapa de _archivado_ del ciclo de vida.
- [[Cumplimiento y Auditoría]]: la retención es un requisito de cumplimiento.
- [[Regulaciones de Protección de Datos]]: las regulaciones que definen los períodos.
- [[GDPR]]: la regulación que limita la retención al mínimo necesario.
- [[Respuesta ante Incidentes]]: los logs retenidos son clave para el análisis forense post-incidente.
