Una **blacklist** (lista negra) es una base de datos que recopila información sobre amenazas de seguridad conocidas, utilizada para bloquear accesos maliciosos.

## Contenido típico

- IPs de origen de ataques registrados.
- Dominios o URLs asociados a malware o phishing.
- Metodologías y software (malware) utilizados en ataques.

## Origen de los datos

Existen distintas iniciativas, tanto **sin fines de lucro** como **comerciales**, que recolectan información sobre ataques recientes y la consolidan en estas bases de datos.

## Uso en sistemas de control de acceso

Las blacklists se integran con sistemas como [[Squid Web Proxy]] para bloquear automáticamente conexiones hacia o desde IPs o dominios listados. Representan una forma de [[Reglas basadas en Contexto]]: el contexto es la reputación conocida del origen/destino.

## Relación con otros conceptos

- [[Squid Web Proxy]]: uno de los sistemas que las consume.
- [[Reglas basadas en Contexto]]: las blacklists son datos contextuales dinámicos.
- [[Mandatory Access Control]]: el bloqueo se aplica obligatoriamente.

## Referencia

https://myip.ms/browse/blacklist/
