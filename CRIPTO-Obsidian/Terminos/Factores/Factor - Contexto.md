El factor **"contexto"** basa la identificación en **datos del entorno** asociados al acceso.
## Ejemplos

- Red de origen / país y ciudad / geolocalización
- Host / terminal / dispositivo de origen
- Día y hora de acceso
- _Velocity_ (velocidad de cambio de ubicación)

## Características

- El contexto puede actuar como **factor positivo** (refuerza la identidad):
  > Un operador debe conectarse desde la red privada del centro de cómputos.

- O como **factor negativo** (genera desconfianza):
  > Si un usuario accede desde otro país, hay _menos_ chances de que sea quien dice ser.

## Uso en detección de anomalías

Permite definir reglas lógicas para detectar situaciones anómalas. Ver: [[Reglas basadas en Contexto]]

Ejemplos:
- Un usuario se loguea desde China y Argentina con 5 minutos de diferencia (_impossible travel_).
- Una usuaria con licencia por maternidad accede a una base de datos.
- Un usuario inicia dos VPNs en menos de 5 minutos desde IPs distintas.

## Relación con otros factores

A diferencia de los otros factores, el contexto no es algo que la entidad _posea_ o _conozca_, sino algo que el sistema **observa del entorno**. Por eso suele usarse como capa adicional en [[Multi-Factor Authentication]].

## Relación con otros conceptos

- [[Factor de Autenticación]]: categoría a la que pertenece.
- [[Reglas basadas en Contexto]]: mecanismo de control de acceso que lo implementa.
- [[Zero-Trust]]: modelo que formaliza el uso del contexto como factor de verificación continua.
- [[Multi-Factor Authentication]]: puede complementar a otros factores.
