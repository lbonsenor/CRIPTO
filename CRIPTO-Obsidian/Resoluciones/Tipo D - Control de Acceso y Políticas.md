Ejemplos: cámaras con operadores/supervisores, control de acceso en un oleoducto.

## Método de resolución

1. Identificar **sujetos** (quién) y **objetos** (qué) — ver [[Sujeto]] y [[Objeto]].
2. Armar una tabla de permisos (Lectura/Escritura/Ninguno) por rol.
3. Elegir el modelo según el foco del ejercicio:
   - ¿Confidencialidad? → [[Modelo Bell-La Padula]] (no read up, no write down)
   - ¿Integridad? → [[Modelo Biba]] (no read down, no write up)
   - ¿Permisos asignados por rol? → [[RBAC]] (el más común en finales)
   - ¿Procesos de transformación regulados? → [[Modelo Clark-Wilson]]
4. Si [[Modelo Biba]] o [[Modelo Bell-La Padula]] no encajan limpiamente con el enunciado → usar [[RBAC]] y justificar con [[Principios de Diseño de Seguridad|los principios de Saltzer y Schroeder]].
5. Siempre mencionar cuando aplique:
   - [[Least Privilege|Menor privilegio]]
   - Separación de tareas ([[Modelo Clark-Wilson]], regla C3)
   - Logs/auditoría ([[Modelo Clark-Wilson]], regla C4) si preguntan por mecanismos compensatorios

## Mecanismos compensatorios comunes

| Problema | Mecanismo |
|---|---|
| Un operador ignora algo a propósito | Redundancia (2+ operadores por recurso) |
| No se puede saber quién hizo qué | Logs/auditoría ([[Modelo Clark-Wilson]] C4) |
| Dependencia del factor humano | Detección automática (alertas) |
| Un operador siempre ve lo mismo | Rotación de asignaciones |
| Una sola persona controla todo | Separación de tareas ([[Modelo Clark-Wilson]] C3) |
