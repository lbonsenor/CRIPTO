Una **vulnerabilidad** es una debilidad en un activo que permite a un atacante lograr acceso no autorizado. Ver [[Amenazas y Vulnerabilidades]].

## Clasificación

### Código fuente deficiente
Deficiencias en el código fuente de aplicaciones, librerías y herramientas. Usualmente son fallas en controles o lógica deficiente.

### Configuración incorrecta
Sistemas configurados de forma incorrecta o con opciones de seguridad insuficientes. Es la forma más común de vulnerabilidad en servicios de nube.

### Relaciones de confianza no validadas
Relaciones que no validan correctamente su uso o no revalidan la autenticidad del sujeto.
- Ejemplo: NFS no valida al usuario antes de dar acceso a los archivos.

### Credenciales y autenticación débil
- Políticas de contraseñas inseguras.
- Exposición de credenciales que facilita su adivinanza.
- Falta de [[Multi-Factor Authentication]].

### Cifrado débil
Uso de algoritmos de cifrado vulnerables que permiten robar o derivar las claves de cifrado. Ver [[Seguridad Computacional]].

### Amenaza interna maliciosa (_insider threat_)
Una persona que incumple las reglas o buenas prácticas con el fin de cometer daño o fraude.

### Error humano e ingeniería social
- Cualquier causa humana que sin intención genera un problema de seguridad.
- Toda forma de **ingeniería social** (phishing, pretexting, etc.).
- Amenazas a empleados para realizar tareas que comprometan la seguridad (_coercion_).
- Procedimientos inadecuados de [[Autenticación]] (no relacionados con credenciales).
- Formas de reset de contraseña débiles (preguntas triviales o información pública).
- Falta de un [[Fail-Safe Defaults|fail-safe]].

### Inyección de código
Cualquier forma de inyección que permite a una aplicación realizar tareas distintas a las programadas. Genera backdoors para ignorar controles de red o de autenticación. Ver [[CWE-89 - SQL Injection]], [[CWE-79 - XSS]], [[CWE-78 - OS Command Injection]].

### Exposición de datos
- Exposición de datos confidenciales por cualquier medio.
- Errores que muestran información interna o credenciales.
- Tachos de basura con documentos confidenciales, impresoras con documentos no reclamados.

### Logging insuficiente
- Insuficiencia en la capacidad de monitorear registros de actividad.
- Falta de visibilidad y alertas sobre actividad sospechosa.
- Generación de eventos sin procesos adecuados de análisis.

### Recursos compartidos sin aislamiento
Cualquier recurso compartido entre organizaciones o usuarios que no aísle correctamente la información. Ejemplo: una computadora compartida que almacena datos de dos usuarios en una carpeta compartida (`/tmp`). Ver [[CWE-416 - Use After Free]].

## Referencia

https://www.spiceworks.com/it-security/vulnerability-management/articles/what-is-a-security-vulnerability/
