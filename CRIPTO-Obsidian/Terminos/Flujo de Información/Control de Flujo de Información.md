El **control de flujo de información** agrupa las técnicas que verifican que la información no se transfiera de manera no autorizada dentro de un sistema. Se aplican en dos momentos distintos.

## Tiempo de compilación (_compile-time enforcement_)

Analiza la propagación de información **directamente en el código fuente** antes de la ejecución.

### Metodología

1. Cuantificar la información asociada a cada variable usando [[Entropía como Medida de Información]].
2. Evaluar cómo cambia esa información en cada sentencia.
3. Comparar el valor inicial y final de cada variable.
4. Si los cambios indican una transferencia no autorizada → **fallo**.

### Tipos de flujo detectados

- [[Flujo Directo]]: asignaciones explícitas entre variables.
- [[Flujo Indirecto]]: flujo por estructuras de control o comportamiento.

### Limitaciones

- **No es computable** verificar todo programa arbitrario (indecidibilidad).
- La interactividad hace explotar el espacio de casos.
- Los **ciclos** son un problema particular (terminación no garantizada).
- Por su costo y complejidad, se limita a **componentes muy sensibles** (ej: verificar que una función de cifrado no revele información de la clave).

## Tiempo de ejecución (_run-time enforcement_)

Control dinámico aplicado durante la ejecución del programa. Es el método más utilizado en la práctica.

| Técnica | Descripción | Archivo |
|---|---|---|
| **Sandboxing** | Control y modificación de las interacciones del proceso con su entorno | [[Aislamiento]] |
| **Virtualización** | Ambiente de ejecución sintético separado del real | [[Aislamiento]] |

## Relación con otros conceptos

- [[Flujo de Información]]: el problema que estas técnicas buscan controlar.
- [[Problema del Confinamiento]]: el objetivo de seguridad que se intenta alcanzar.
- [[Aislamiento]]: las técnicas de ejecución más utilizadas.
