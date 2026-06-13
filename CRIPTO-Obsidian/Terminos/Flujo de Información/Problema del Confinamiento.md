El **problema del confinamiento** se refiere a la dificultad de asegurar que un proceso (sujeto) **no transmita información** a otra entidad (objeto) a la que no está autorizado a acceder, ya sea de forma directa o indirecta.

## Motivación

Si se pudiera confinar el flujo de información dentro de un ambiente controlado, no habría riesgos de confidencialidad o integridad. El [[Modelo Bell-La Padula]] define un modelo de seguridad orientado a asegurar este confinamiento.

## Modelo ideal: aislación total

| Requisito | Consecuencia |
|---|---|
| Un proceso no puede comunicarse con otros | No revela información directamente |
| Un proceso no puede ser observado | No revela información indirectamente |

### El problema en la práctica

En la práctica, los procesos usan **recursos observables** que pueden filtrar información:

- Memoria (patrones de acceso, tamaño ocupado)
- Ciclos de CPU (carga, tiempo de respuesta)
- Espacio en disco
- Ancho de banda de red

Estos recursos observables son la base de los [[Covert Channel|canales encubiertos]] y los [[Side-Channel Attack|ataques de canal lateral]].

## Relación con otros conceptos

- [[Modelo Bell-La Padula]]: modelo de seguridad diseñado para el confinamiento.
- [[Flujo de Información]]: el marco general del que forma parte.
- [[Covert Channel]]: explotación de recursos observables para transmitir información oculta.
- [[Aislamiento]]: técnicas prácticas para aproximarse al modelo ideal.
