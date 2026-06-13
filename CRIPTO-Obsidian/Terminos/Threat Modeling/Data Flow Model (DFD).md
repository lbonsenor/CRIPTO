El **Diagrama de Flujo de Datos** (_Data Flow Diagram_) es un modelo para representar las partes de un sistema y los riesgos de cada una de ellas. Se usa en conjunto con [[Modelo STRIDE]] dentro del proceso de [[Threat Modeling]].

## Elementos del DFD

| Elemento | Descripción | Ejemplos |
|---|---|---|
| **Procesos** | Componentes que procesan datos | Contenedores, procesos ejecutables, microservicios |
| **Entidades externas** | Actores fuera del sistema | Personas, otros sistemas |
| **Flujos de datos** | Comunicaciones entre componentes | APIs, mensajes, llamadas HTTP |
| **Almacenamientos** | Donde persisten los datos | Archivos, bases de datos, secretos, caches |

## Aplicación con STRIDE

Para cada elemento del DFD, se cruza con las categorías de [[Modelo STRIDE]] para identificar amenazas aplicables:

```
Por cada elemento del DFD:
  Para cada categoría de STRIDE:
    ¿Aplica esta amenaza a este elemento?
    → Si sí: definir gestión del riesgo
```

## Gestión de riesgos identificados

Por cada riesgo identificado se debe definir cómo gestionarlo:

- **Eliminar**: rediseñar para que la amenaza no aplique.
- **Mitigar**: reducir la probabilidad o el impacto.
- **Compensar**: agregar controles alternativos.
- **Aceptar**: documentar y asumir el riesgo conscientemente.

## Referencia

https://www.researchgate.net/publication/228793485_Experiences_Threat_Modeling_at_Microsoft
