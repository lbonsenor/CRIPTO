![[2023-Q1-1-EJ3.png]]
**Conceptos:**
- [[Factor de Autenticación]]
- [[PostgreSQL - Policies]]
### En sistemas de control de acceso que incluyen agrupamientos de usuarios, la resolución de conflictos en la asignación de los permisos de GRANT-ALL implica darle a todos los usuarios posibles los permisos.
**FALSO**
`GRANT ALL` es una sentencia/comando que se utiliza para otorgar **todos los privilegios disponibles** sobre un objeto a un usuario o rol determinado

### Los esquemas de autenticación por varios factores ofrecen esquemas más seguros de romper debido a que se requiere comprometer de manera simultanea varios canales de autenticación
**VERDADERO**

### En Bell-Lapadula la condición de seguridad simple (CSS) implica que un usuario puede leer un objeto si y solo si su nivel es igual o mayor que el del objeto.
**VERDADERO**

### En un sistema sensible de generación de claves simétricas, si la entropía de esa clave es 256 bits, pero la entropía de esa clave se reduce a 200 bits si se registra el sonido del disipador de hardware, ¿Hay flujo de información de una variable a la otra?
**SÍ, HAY FLUJO DE INFORMACIÓN.**
Existe un ataque de canal lateral (side-channel attack). La reducción de la entropía de la clave (de 256 a 200 bits) al conocer una variable física (el sonido del disipador) demuestra que ambas variables no son estadísticamente independientes
