**Data Governance** son todas las tareas realizadas para asegurar que los datos estén **seguros, privados e íntegros**, y que estén disponibles y puedan usarse.

Incluye las medidas que deben tomar las **personas**, los **procesos** que deben seguir y la **tecnología** que los respalda — a lo largo del ciclo de vida completo de los datos.

## Componentes

| Componente | Descripción | Archivo |
|---|---|---|
| **Data Life Cycle** | Ciclo de vida de los datos desde creación hasta eliminación | [[Data Life Cycle]] |
| **Regulaciones** | Marco legal y normativo externo | [[Regulaciones de Protección de Datos]] |
| **Data Roles** | Roles dentro de la organización para el uso de la información | [[Data Roles]] |
| **Clasificación** | Identificación y etiquetado de datos por sensibilidad | [[Clasificación de Datos]] |
| **Data Inventory** | Registro de todos los datos críticos y sus responsables | [[Data Inventory]] |
| **Data Labeling** | Inventario embebido en los propios datos como metadata | [[Data Labeling]] |
| **Estados de los datos** | At-rest, in-motion, in-use | [[Estados de los Datos]] |

## Ciclo de protección

La protección de datos no es un control puntual sino un **proceso continuo** con cuatro fases:

```
Govern → Discover → Protect → Comply → Detect → Respond
```

| Fase | Descripción |
|---|---|
| **Govern** | Definir políticas, roles y estándares → [[Data Governance]] |
| **Discover** | Clasificar, inventariar y etiquetar datos → [[Clasificación de Datos]] |
| **Protect** | Aplicar controles técnicos: cifrado, enforcement → [[Cifrado en Reposo]], [[Cifrado en Tránsito]] |
| **Comply** | Cumplir regulaciones, auditorías y retención → [[Regulaciones de Protección de Datos]], [[Retención de Información]] |
| **Detect** | Detectar pérdidas o fugas → [[DLP - Data Loss Prevention]] |
| **Respond** | Responder a incidentes → [[Respuesta ante Incidentes]] |

## Relación con otros conceptos

- [[Control de Acceso]]: necesario pero **no suficiente** — los controles de acceso no evitan que datos legítimamente accedidos sean filtrados.
- [[La triada]]: Data Governance protege las tres dimensiones — confidencialidad, integridad y disponibilidad.
- [[Gobernanza]]: Data Governance es la aplicación específica de la gobernanza al activo "datos".
- [[Flujo de Información]]: la base teórica de por qué los controles de acceso solos no alcanzan.

## Referencia

https://www.pwc.com/ph/en/risk-assurance/risk-assurance-it/data-protection-privacy-ra-it.html
