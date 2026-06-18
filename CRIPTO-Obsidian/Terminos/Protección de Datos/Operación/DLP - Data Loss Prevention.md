Las herramientas de **DLP** (_Data Loss Prevention_) ayudan en el proceso de clasificación, inventariado y **detección de información en todos sus estados** ([[Estados de los Datos]]).

## Características

- Son herramientas **detectivas**, no preventivas ni correctivas: se activan cuando se produce una falla en la aplicación de una política.
- No evitan directamente la pérdida de datos — **alertan y registran** para que pueda actuarse.
- Son **complejas de implementar** porque requieren un esfuerzo de toda la organización (no solo de IT).

## Evolución

Históricamente, las herramientas DLP fueron difíciles de implementar de forma independiente. Con el tiempo, las funcionalidades de DLP fueron **incorporadas a productos que las empresas ya usan**:

| Plataforma | Integración DLP |
|---|---|
| Microsoft (Exchange, SharePoint, Teams) | Microsoft Purview DLP |
| Google Workspace | Google DLP |
| Endpoints (USB, CD/DVD, impresoras) | DLP de endpoint (ej: Symantec, Forcepoint) |

Las empresas que nacieron haciendo DLP de forma independiente fueron compradas e incorporadas a estas soluciones más amplias.

## Lo que monitorea

- Transferencias de archivos a medios extraíbles (USB, CD/DVD).
- Emails con adjuntos o contenido sensible.
- Impresiones de documentos confidenciales.
- Subidas a servicios cloud no autorizados.
- Tráfico de red con datos sensibles.

## Relación con otros conceptos

- [[Enforcement]]: el DLP es la implementación del enforcement en tiempo de ejecución a nivel de infraestructura.
- [[Clasificación de Datos]]: el DLP usa la clasificación para saber qué detectar.
- [[Data Inventory]]: el inventario alimenta las reglas del DLP.
- [[Respuesta ante Incidentes]]: el DLP genera las alertas que disparan la respuesta.
- [[Flujo de Información]]: el DLP es una implementación práctica del control de flujo.

## Referencia

https://www.gartner.com/en/articles/build-a-successful-data-loss-prevention-program-in-5-steps
