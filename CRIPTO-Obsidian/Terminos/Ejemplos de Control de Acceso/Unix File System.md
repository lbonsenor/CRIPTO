El sistema de archivos Unix implementa un modelo de [[Discretionary Access Control]] (DAC) donde el **dueño del archivo** define los permisos de acceso.

## Modelo de permisos

Los permisos se organizan en tres dimensiones:

| Sujeto | Permisos posibles |
|---|---|
| **Owner** (dueño) | Read (r), Write (w), Execute (x) |
| **Group** (grupo) | Read (r), Write (w), Execute (x) |
| **Others** (resto) | Read (r), Write (w), Execute (x) |

## umask

El **umask** define los permisos por defecto que se **restan** al crear nuevos archivos y directorios. Controla los permisos iniciales de los objetos recién creados.

## Características

- Los permisos **no se heredan** automáticamente del directorio padre al crear nuevos archivos (a diferencia de [[Windows File System]]).
- Es la base del DAC en sistemas Linux/Unix.
- Puede complementarse con [[SELinux]] para agregar una capa de [[Mandatory Access Control]].

## Relación con otros conceptos

- [[Discretionary Access Control]]: el dueño decide los permisos.
- [[SELinux]]: extiende el modelo con MAC.
- [[Control por Extensión]]: las ACLs de Linux extienden el modelo básico con listas más granulares.

## Referencias

- https://phoenixnap.com/kb/what-is-umask
