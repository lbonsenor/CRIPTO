El **data labeling** es un modelo alternativo a [[Data Inventory|el inventario centralizado]] que busca **inventariar los datos dentro de los propios datos**, como metadata.

## Concepto

En lugar de mantener un registro externo de cada dato y su clasificación, la [[Clasificación de Datos|clasificación]] y los metadatos de gobernanza se embeben **directamente en el dato** (o su contenedor) como etiquetas.

## Ventajas

- **Descentraliza el inventario**: las etiquetas circulan con los datos, sin importar dónde estén almacenados.
- **Simplifica las reglas** de protección y monitoreo: los sistemas leen la etiqueta del dato para saber cómo tratarlo.
- **Reduce el riesgo de desactualización**: la etiqueta viaja con el dato a donde sea que migre.

## Limitaciones

- **No es un proceso estandarizado**: distintos sistemas usan distintos formatos de etiqueta, por lo que deben traducirse entre medios.
- Los sistemas receptores deben ser capaces de leer e interpretar las etiquetas.

## Ejemplo: AWS Tag Policy

En AWS, las etiquetas (_tags_) son pares clave-valor que se aplican a cualquier recurso:

```
Recurso: S3 bucket "datos-clientes"
Tags:
  classification: confidential
  data-owner:     finance-team
  retention:      7-years
```

Las políticas de la organización (AWS Tag Policy) pueden exigir que todos los recursos tengan ciertas etiquetas, y las herramientas de [[CSPM]] o [[DLP - Data Loss Prevention]] pueden usar esas etiquetas para aplicar controles automáticamente.

## Relación con otros conceptos

- [[Data Inventory]]: el inventario centralizado que el data labeling complementa o reemplaza.
- [[Clasificación de Datos]]: el contenido que las etiquetas registran.
- [[Data Governance]]: el proceso que define qué etiquetar y cómo.
- [[CSPM]]: herramientas que usan las etiquetas para auditar la postura de seguridad cloud.
