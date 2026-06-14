# Cifrado en Tránsito

El **cifrado en tránsito** (_encryption in-transit_) protege datos que se mueven activamente de una ubicación a otra, evitando su intercepción.

Ver [[Estados de los Datos]] para el contexto general.

## Enfoques

### Cifrar el dato antes de enviarlo

El dato se cifra en origen antes de transmitirse, independientemente del protocolo de transporte.

- Ejemplo: cifrar un archivo con **GPG** antes de enviarlo por email.
- El canal puede ser inseguro; la seguridad está en el dato mismo.

### Usar un protocolo de transporte que soporte cifrado

El canal de comunicación se cifra en su totalidad:

| Protocolo | Uso |
|---|---|
| **TLS** (_Transport Layer Security_) | HTTPS, SMTP con TLS, IMAPS, etc. |
| **IPSec** | Cifrado a nivel de red, usado en VPNs |
| **SSH** | Acceso remoto seguro a servidores |

## Enforcement en tiempo de ejecución

El cumplimiento del cifrado en tránsito puede verificarse y forzarse en tiempo de ejecución mediante [[DLP - Data Loss Prevention]] u otras herramientas de infraestructura que monitoren el tráfico de red.

Ver: [[Enforcement Execution-time]]

## Relación con otros conceptos

- [[Estados de los Datos]]: el estado "in-motion" que protege.
- [[Intercambio de Claves]]: mecanismo para establecer la clave de sesión en TLS/IPSec.
- [[Criptosistema Asimétrico]]: usado en el handshake de TLS para acordar la clave simétrica.
- [[CBC]], [[CTR]], [[AES]]: los modos de cifrado simétrico usados dentro de TLS para el payload.
- [[Certificado Digital]]: TLS usa certificados para autenticar al servidor.
