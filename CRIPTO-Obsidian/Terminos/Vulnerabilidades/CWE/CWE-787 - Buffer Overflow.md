**CWE-787** representa la escritura en memoria **fuera de los límites** originalmente definidos para un buffer.

Forma más conocida: **Buffer Overflow**.

## Descripción

Ocurre cuando un programa escribe datos más allá del límite de un buffer asignado en memoria. El exceso de datos sobrescribe regiones de memoria adyacentes, que pueden contener variables de programa, punteros de retorno o datos del sistema.

## Consecuencias

- Corrupción de memoria que produce comportamiento indefinido o crash.
- En escenarios controlados por un atacante: **ejecución arbitraria de código**.
- Escalada de privilegios si se sobrescribe información sensible del proceso.

## Ejemplo conceptual

```c
char buf[8];
// El input del usuario no se valida
strcpy(buf, input_del_usuario); // Si input > 8 bytes → overflow
```

## Prevención

- Usar funciones de manejo de strings seguras (`strncpy`, `snprintf`).
- Validar longitudes de input antes de copiar.
- Lenguajes con gestión automática de memoria (Java, Python, Rust) eliminan esta clase de error.
- Técnicas como ASLR, stack canaries y DEP/NX reducen la explotabilidad.

## Referencia

https://cwe.mitre.org/data/definitions/787.html
