# Estados de los Datos

Los datos existen en tres estados distintos, cada uno con sus propios riesgos y mecanismos de protección.

## Los tres estados

### Data at-rest (en reposo)

Datos que **no se mueven activamente** — almacenados en disco duro, laptop, unidad flash u otro medio de almacenamiento.

- **Riesgo principal**: acceso físico al dispositivo o compromiso del sistema de archivos.
- **Controles**: cifrado del almacenamiento, [[HSM]], eliminación segura.
- Ver: [[Cifrado en Reposo]]

### Data in-motion (en tránsito)

Datos que **se mueven activamente** de una ubicación a otra — a través de internet, red privada, o entre componentes internos.

- **Riesgo principal**: intercepción en tránsito (man-in-the-middle).
- **Controles**: TLS, IPSec, SSH, GPG.
- Ver: [[Cifrado en Tránsito]]

### Data in-use (en uso / en memoria)

Datos que están siendo **procesados activamente** — en RAM, en registros de CPU, en caché.

- **Riesgo principal**: acceso desde otros procesos o VMs que comparten el mismo hardware.
- **Controles**: cifrado de memoria (TME, TME-MK).
- Ver: [[Cifrado en Memoria]]

## Un mismo dato en distintos estados

El mismo dato puede existir en múltiples estados simultáneamente:

```
Base de datos (at-rest) → API que lo consulta (in-motion) → Memoria del servidor (in-use)
```

Los controles deben cubrir **todos los estados** — proteger solo uno deja ventanas de exposición.

## Relación con otros conceptos

- [[Data Governance]]: los estados son la dimensión "dónde está el dato" de la gobernanza.
- [[Data Life Cycle]]: los estados se corresponden con etapas del ciclo de vida.
- [[DLP - Data Loss Prevention]]: monitorea los datos en todos sus estados.
- [[Flujo de Información]]: la base teórica del problema de proteger datos en movimiento.
