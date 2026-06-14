# Data Life Cycle

El **ciclo de vida de los datos** describe las etapas por las que atraviesa la información desde su creación hasta su eliminación. Los controles de [[Data Governance]] deben aplicarse en **cada etapa** del ciclo.

## Etapas

```
Creación / Captura → Almacenamiento → Uso → Compartición → Archivado → Eliminación
```

| Etapa | Descripción | Controles relevantes |
|---|---|---|
| **Creación / Captura** | Generación de nuevos datos o ingesta desde fuentes externas | [[Clasificación de Datos]], [[Data Labeling]] |
| **Almacenamiento** | Persistencia en bases de datos, archivos, nube | [[Cifrado en Reposo]], [[Data Inventory]] |
| **Uso** | Procesamiento y acceso por aplicaciones y usuarios | [[Control de Acceso]], [[Cifrado en Memoria]], [[Enforcement Compiler-time]] |
| **Compartición** | Transferencia entre sistemas, personas u organizaciones | [[Cifrado en Tránsito]], [[DLP - Data Loss Prevention]] |
| **Archivado** | Retención de datos históricos o regulatorios | [[Retención de Información]] |
| **Eliminación** | Destrucción segura de datos que ya no son necesarios | Secure Deletion, Data Remanence |

## Implicaciones de seguridad

Un mismo dato puede existir en **distintos estados simultáneamente** en distintos sistemas:

- La copia en disco → [[Cifrado en Reposo]]
- La copia en tránsito → [[Cifrado en Tránsito]]
- La copia en RAM → [[Cifrado en Memoria]]

Ver: [[Estados de los Datos]]

## Relación con otros conceptos

- [[Data Governance]]: el ciclo de vida es la dimensión temporal de la gobernanza.
- [[Data Inventory]]: registra dónde está cada dato en cada etapa del ciclo.
- [[Regulaciones de Protección de Datos]]: muchas regulaciones definen requisitos por etapa (ej: cuánto tiempo retener, cómo eliminar).
