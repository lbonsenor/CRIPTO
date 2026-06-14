# Clasificación de Datos

La **clasificación de datos** es el proceso de identificar, categorizar y etiquetar los datos según su sensibilidad, para aplicar los controles de protección adecuados a cada categoría.

## Proceso

| Fase | Actividades |
|---|---|
| **Planificar** | Identificar los activos de datos. Identificar los roles sobre esos datos ([[Data Roles]]). Definir perfiles de acceso. |
| **Ejecutar** | Educar a los involucrados. Desplegar la tecnología de clasificación y etiquetado ([[Data Labeling]]). |
| **Monitorear** | Revisar logs y alertas. |
| **Reaccionar** | Reclasificar datos. Bloquear o permitir accesos. |

## Niveles de clasificación típicos

| Nivel | Descripción | Ejemplo |
|---|---|---|
| **Público** | Sin restricción de acceso | Comunicados de prensa, sitio web |
| **Interno** | Solo para empleados | Procedimientos internos |
| **Confidencial** | Acceso restringido a roles específicos | Datos de clientes, contratos |
| **Secreto / Crítico** | Acceso muy restringido, máxima protección | Claves de cifrado, datos de tarjetas (PCI-DSS), datos de salud (HIPAA) |

## Relación con los estados de los datos

La clasificación debe considerarse en **todos los estados** del dato:

- **At-rest**: el nivel de cifrado y control de acceso al almacenamiento.
- **In-motion**: el protocolo de transporte requerido.
- **In-use**: los controles de acceso en memoria.

Ver: [[Estados de los Datos]]

## Relación con otros conceptos

- [[Data Governance]]: la clasificación es la fase _Discover_ del ciclo de gobernanza.
- [[Data Inventory]]: el inventario donde se registra la clasificación de cada dato.
- [[Data Labeling]]: la técnica de embeber la clasificación en los propios datos.
- [[Mandatory Access Control]]: modelo de control de acceso que utiliza niveles de clasificación.
- [[Modelo Bell-La Padula]]: modelo formal que define reglas de flujo basado en niveles de clasificación.
