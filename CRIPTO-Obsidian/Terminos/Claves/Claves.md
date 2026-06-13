Las **claves** (contraseñas) son la implementación más común del [[Factor - Algo que conozco]].

## Tipos de claves

| Tipo | Descripción |
|---|---|
| **Secuencias de caracteres** | Con o sin restricciones (longitud, tipos de caracteres). Generadas aleatoriamente, por el usuario, o de forma asistida. |
| **Frases clave** (_passphrase_) | Secuencias de palabras. Más fáciles de recordar y con alto espacio de claves. |
| **Algorítmicas** | Derivadas por algún algoritmo. |
| **Challenge-Response** | Ver [[Challenge-Response]]. |
| **OTP** (_One Time Password_) | Claves de uso único. Ver [[OTP - One Time Password]]. |

## Almacenamiento

### ❌ Texto en claro
- En archivo o base de datos.
- Puede ser accedido desde fuera del sistema.
- **NO RECOMENDADO**: no puede garantizarse confidencialidad.

### ⚠️ Archivo cifrado
- Requiere una clave maestra para acceder a las contraseñas.
- La clave maestra puede estar en un archivo de configuración, en el ejecutable, ingresarse al inicio, o en un dispositivo criptográfico especializado.
- Solo recomendado si es requisito recuperar la clave original (ej: reenviarla a un tercer sistema).

### ✅ Hash con Salt + PBKDF2
- **La forma correcta** de almacenar contraseñas.
- Ver [[Salting]] y [[PBKDF2]].

## Selección de claves

| Estrategia | Ventaja | Desventaja |
|---|---|---|
| **Aleatoria** | Espacio de claves máximo; equiprobable | Difícil de memorizar (ej: `fL3K&8%j"`) |
| **Pronunciable** | Fácil de memorizar (ej: `helgoret`) | Espacio de claves muy reducido |
| **Elegida por el usuario** | Cómoda para el usuario | Tendencia a ser predecible |

## Políticas de gestión

- [[Revisión Proactiva de Claves]]: analizar la clave al crearla y rechazar las débiles.
- [[Expiración de Claves]]: forzar el cambio periódico.

## Estimación de seguridad

Ver [[Fórmula de Anderson]] para calcular el tiempo estimado de un ataque de fuerza bruta.

## Relación con otros conceptos

- [[Sistema de Autenticación]]: las claves son el tipo `A` más común.
- [[Salting]]: técnica para reforzar el almacenamiento.
- [[PBKDF2]]: función estándar para derivar el hash almacenado.
- [[Ataques a Sistemas de Autenticación]]: vectores de ataque sobre claves.
