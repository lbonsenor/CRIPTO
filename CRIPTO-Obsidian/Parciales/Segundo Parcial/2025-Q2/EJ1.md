![[2-2025-Q2-EJ1.png]]
**Conceptos**
- [[Modelo Bell-La Padula]]
- [[Control de Acceso]]
- [[Políticas de Seguridad]]
- [[Modelo de Seguridad]]
### a)
Para manejar secretos militares, el estándar de la industria es un modelo de **Control de Acceso Mandatorio (MAC)** formal, específicamente el modelo **Bell-LaPadula**, enfocado en preservar la confidencialidad.

**Asignación de Niveles de Autorización (Clearance):** Se etiqueta a los sujetos (ingenieros, analistas) y objetos (planos, código de drones) con niveles jerárquicos: `No clasificado < Confidencial < Secreto < Top Secret`
**Aislamiento por Compartimentos (Necesidad de saber):** Cada proyecto militar se asigna a una categoría cerrada (ej. `{Ejercito_EEUU}`, `{Ejercito_OTAN}`, `{Ejercito_Uk}`). Un ingeniero solo puede acceder a un objeto si su perfil incluye explícitamente esa categoría.

**Propiedad de Simple Confidencialidad ("No leer hacia arriba"):** Un usuario con nivel _Secreto_ no puede leer un documento etiquetado como _Top Secret_.
    
**Propiedad Estrella (*) de Confinamiento ("No escribir hacia abajo"):** Un ingeniero que está trabajando con datos _Top Secret_ no puede escribir o exportar información hacia un archivo de nivel _Secreto_ o inferior. Esto evita la fuga accidental o deliberada de secretos de estado hacia canales menos seguros.

### b)
**Modelo de Seguridad**: Es una representación matemática, formal o abstracta de un objetivo de seguridad. Define formalmente los estados seguros de un sistema y demuestra que, matemáticamente, los datos no se van a filtrar o alterar.

**Control de Acceso**: Es la declaración de intenciones de alto nivel. Define el "qué se quiere proteger" y "quiénes tienen derecho a qué" según las necesidades del negocio o de la organización (las reglas de juego institucionales)

**Reglas de Seguridad:** Son los mecanismos y algoritmos concretos que se programan en el sistema o software para forzar el cumplimiento del modelo y la política.