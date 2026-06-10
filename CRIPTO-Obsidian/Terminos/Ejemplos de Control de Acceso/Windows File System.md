El sistema de archivos de Windows (NTFS) implementa un modelo de [[Discretionary Access Control]] donde el **dueño de cada archivo o directorio** define los permisos.

## Características del modelo

- Los **sujetos** pueden ser cuentas individuales o grupos.
- Existen dos niveles de configuración:
  - **Básico**: permisos simplificados (leer, escribir, control total, etc.).
  - **Avanzado**: más granular, permite control fino sobre operaciones específicas.

## Herencia de permisos

- Los permisos se pueden **heredar** de un directorio padre a su contenido.
- La relación de herencia **persiste en la creación** de nuevos archivos, a diferencia de [[Unix File System]] donde los permisos no se heredan automáticamente.

## Resolución de permisos efectivos

Cuando existen muchas reglas superpuestas, Windows calcula los **permisos efectivos** resultantes:

1. **Deny tiene precedencia sobre Allow** → [[Deny over Allow]].
2. Las **reglas locales** tienen precedencia sobre las **reglas heredadas**.
3. Se puede usar un simulador ("Permisos Efectivos" en _Advanced Security Settings_) para entender los permisos reales de un sujeto.

## Relación con otros conceptos

- [[Discretionary Access Control]]: el dueño controla los permisos.
- [[Deny over Allow]]: regla de resolución aplicada por defecto.
- [[Unix File System]]: sistema análogo en Linux/Unix.

## Referencia

https://learn.microsoft.com/en-us/training/modules/configure-manage-file-access/
