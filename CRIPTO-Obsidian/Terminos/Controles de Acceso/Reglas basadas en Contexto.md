
Las **reglas basadas en contexto** son un tipo de [[Control por Comprensión]] donde las condiciones de acceso incluyen datos asociados al sujeto, al objeto, o al momento y lugar del acceso.

## ¿Qué es el contexto?

El contexto representa **elementos que pueden cambiar independientemente de las reglas de acceso**, como:

- Momento del día.
- Ubicación geográfica del origen de la conexión.
- Departamento al que pertenece el sujeto.
- Estado del sujeto (ej: de vacaciones, de licencia).
- Nivel de confiabilidad del dispositivo desde el que se conecta.

## Ejemplos de reglas contextuales

Permiten detectar **situaciones anómalas**, por ejemplo:

- Un usuario se loguea desde China y desde Argentina con 5 minutos de diferencia (_impossible travel_).
- Una usuaria con licencia por maternidad accede a una base de datos en el datacenter.
- Un usuario inicia dos VPNs en menos de 5 minutos con dos IPs distintas.

## Dificultades de implementación

- Su implementación muchas veces depende de **lógica que debe programarse** a medida.
- En sistemas que no pueden modificarse, **no es posible incorporar esta lógica**.
- El modelo [[Zero-Trust]] resuelve esto mediante un **Policy Engine externo** (como [[Open Policy Agent]]) que evalúa todas las condiciones de contexto.
