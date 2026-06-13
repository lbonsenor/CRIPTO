**CWE-416** ocurre cuando un programa utiliza un recurso (típicamente memoria) **después de haberlo liberado**, sin haberlo limpiado (_wiped_) entre usos de distintos usuarios o contextos.

## Descripción

En C/C++, cuando se libera memoria con `free()` y luego se accede a esa misma dirección de memoria (sin haberla reescrito), el programa puede leer datos del uso anterior. En un entorno multiusuario o multitenancy, esto puede exponer datos de otro usuario.

## Consecuencias

- Lectura de datos residuales que pertenecían a otro usuario o proceso.
- En escenarios controlados: ejecución arbitraria de código.
- En sistemas multitenancy: filtración de datos entre tenants.

## Ejemplo en sistemas multiusuario

> Una computadora compartida por dos usuarios que almacena información de cada uno en `/tmp` sin limpiar los archivos entre sesiones.

## Prevención

- Limpiar (_wipe_) la memoria y los recursos antes de liberarlos o reutilizarlos.
- Usar lenguajes con gestión automática de memoria.
- En sistemas multiusuario, asegurar el aislamiento de recursos temporales.
- Aplicar [[Least Privilege]] y [[Separation of Privilege]] para limitar el acceso a recursos compartidos.

## Referencia

https://cwe.mitre.org/data/definitions/416.html
