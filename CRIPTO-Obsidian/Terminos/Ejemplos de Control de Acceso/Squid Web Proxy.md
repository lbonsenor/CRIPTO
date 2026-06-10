**Squid** es un web proxy que permite controlar y filtrar la navegación de los usuarios, además de cumplir funciones de caché para acelerar la navegación y reducir el consumo de ancho de banda.

## Funciones principales

- **Control de acceso**: define qué sitios están permitidos o bloqueados.
- **Caché**: almacena respuestas de sitios frecuentes para acelerar el acceso.
- **Reducción de ancho de banda**: evita descargar el mismo contenido múltiples veces.

## Sistema de reglas (ACL)

Squid implementa un sistema de reglas de tipo [[Mandatory Access Control]] (aunque comúnmente llamado "ACL" en su documentación), con reglas ordenadas que filtran por:

- Dominio del sitio
- Protocolo
- URL
- Headers del requerimiento
- Listas negras ([[Blacklist]])

La resolución sigue el criterio de **[[First Match]]**: la primera regla que coincide se aplica.

## Ejemplo de uso real

> En la facultad, los sitios de streaming están bloqueados usando este tipo de tecnología.

## Blacklists

Squid puede integrarse con [[Blacklist|blacklists]] externas para bloquear IPs o dominios asociados a ataques o malware conocidos.

## Relación con otros conceptos

- [[Mandatory Access Control]]: las reglas se aplican a todos los accesos.
- [[RuBAC]]: el sistema de reglas de Squid es una implementación de control basado en reglas.
- [[First Match]]: política de resolución usada por Squid.
- [[Blacklist]]: fuente de datos contextual para bloquear amenazas.

## Referencia

https://medium.com/@minigear/introduction-to-squid-proxy-server-and-application-5f307245fa1d
