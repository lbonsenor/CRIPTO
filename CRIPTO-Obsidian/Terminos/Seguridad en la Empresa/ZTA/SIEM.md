**Security Information and Event Management (SIEM)** es una plataforma que centraliza la **recolección, correlación y análisis** de eventos de seguridad provenientes de múltiples fuentes en la organización.

## Función

- Recolecta logs de sistemas, aplicaciones, dispositivos de red, firewalls, endpoints, etc.
- **Correlaciona eventos** de distintas fuentes para detectar patrones de ataque que no serían visibles en ninguna fuente individual.
- Genera **alertas** ante comportamientos anómalos o indicadores de compromiso.
- Provee un registro histórico para análisis forense post-incidente.

## Rol en ZTA

Dentro de la [[Zero Trust Architecture]], el SIEM alimenta al motor de políticas con información en tiempo real sobre la postura de seguridad del sistema, contribuyendo a las decisiones de acceso dinámicas.

Ver: [[ZTA - Componentes]]

## Casos de uso

- Detección de [[Malware Attack Cycle|movimiento lateral]] en la red.
- Alertas de accesos fuera de horario o desde ubicaciones inusuales. Ver [[Factor - Contexto]].
- Correlación de múltiples intentos de login fallidos ([[Ataques a Sistemas de Autenticación]]).
- Detección de exfiltración de datos a través de canales inusuales. Ver [[Covert Channel]].

## Relación con otros conceptos

- [[ZTA - Componentes]]: el SIEM es uno de los componentes de datos de ZTA.
- [[Defense in Depth]]: el SIEM es la capa de monitoreo de la defensa en profundidad.
- [[Malware Attack Cycle]]: el SIEM es la principal herramienta para detectar las etapas del ciclo.
- [[Reglas basadas en Contexto]]: los eventos del SIEM proveen el contexto para reglas dinámicas.
