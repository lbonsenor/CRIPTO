Un **canal encubierto** (_covert channel_) es un método de comunicación que transfiere información de manera oculta, aprovechando vías **no diseñadas para la transmisión de datos**, con el objetivo de evadir los controles de seguridad.

## Tipos

### Canales de Temporización (_Timing Channels_)

Codifican datos en la **variación del tiempo** entre eventos del sistema.

| Vector | Descripción |
|---|---|
| Latencia de red | Manipular tiempos de respuesta para codificar bits |
| Carga de CPU | Variar la carga para que sea observable desde otro proceso |
| Tiempos de respuesta de aplicaciones | Aprovechar diferencias de timing en operaciones |

### Canales de Almacenamiento (_Storage Channels_)

Usan el almacenamiento en áreas **no convencionales o no reguladas** del sistema.

| Vector | Descripción |
|---|---|
| Espacio de relleno en archivos | Bits no utilizados en el padding de un archivo |
| Atributos de archivos | Codificar datos en fechas de creación, modificación o acceso |
| Bits no usados en estructuras de datos | Campos reservados o padding en protocolos y formatos |

## Ejemplos reales

- **Exfiltración por sonido de impresora**: codificar información en los sonidos generados durante el proceso de impresión. https://dl.acm.org/doi/10.1145/3510583
- **ICMP & DNS Tunneling**: encapsular datos en consultas DNS o paquetes ICMP para exfiltrar información a través de firewalls. https://www.akamai.com/blog/news/introduction-to-dns-data-exfiltration
- **Emanaciones electromagnéticas**: capturar las emisiones del hardware para reconstruir información procesada.

## Diferencia con Side-Channel Attack

| | [[Covert Channel]] | [[Side-Channel Attack]] |
|---|---|---|
| Objetivo | Transmitir información de forma encubierta | Extraer información sensible (ej: claves) |
| Agente | Dos procesos colaboran (uno envía, otro recibe) | Un atacante externo observa el sistema |
| Contexto | Evasión de controles de seguridad internos | Criptoanálisis y espionaje de hardware |

## Relación con otros conceptos

- [[Problema del Confinamiento]]: el covert channel es la consecuencia de que el confinamiento perfecto no existe.
- [[Spyware]]: el malware de tipo spyware suele establecer un covert channel para exfiltrar información.
- [[Side-Channel Attack]]: concepto relacionado pero con distinto modelo de amenaza.
