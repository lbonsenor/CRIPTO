![[2025-Q1-R-EJ2.png]]
En el modo ECB, cada bloque de texto plano se cifra de forma **completamente independiente** utilizando la misma clave secreta.

Esto significa que:

> **Bloques idénticos de texto plano producirán siempre bloques idénticos de texto cifrado.**

Si un mensaje contiene patrones repetitivos (como espacios en blanco, cabeceras de archivos, estructuras de bases de datos o píxeles del mismo color en una imagen), esos patrones se mantendrán perfectamente visibles en el archivo cifrado.