## Para ejercicios de criptografía

1. Identificar el ataque: "El ataque es [nombre] porque [razón específica]".
2. Explicar por qué funciona: "Esto es posible porque [propiedad que falta]".
3. Proponer solución: "Para evitarlo, se modifica [qué] agregando [qué]".
4. Mostrar la estructura resultante: "El mensaje queda: [estructura nueva]".
5. Explicar por qué la solución funciona: "El atacante no puede [qué] porque [razón]".
6. Justificar con teoría: "Esto se alinea con [principio]".

## Para ejercicios de pentesting

1. Nombre: título corto y específico.
2. Hipótesis: vinculada al enunciado, citando componentes concretos.
3. Prueba: acción exacta, input exacto, URL exacta.
4. *(Opcional)* Impacto: qué puede hacer el atacante.

## Conectores teóricos que suman puntos

| Concepto                                          | Cuándo usarlo                                              |
| ------------------------------------------------- | ---------------------------------------------------------- |
| [[Least Privilege]] (Saltzer y Schroeder)         | Cuando un componente tiene más información de la necesaria |
| Separación de tareas ([[Modelo Clark-Wilson]] C3) | Cuando una sola persona/componente controla todo un flujo  |
| Logs/auditoría ([[Modelo Clark-Wilson]] C4)       | Cuando se necesita detectar fraude a posteriori            |
| [[Fail-Safe Defaults]]                            | Cuando hay un trade-off entre disponibilidad y seguridad   |
| No repudio → [[Firma Digital]]                    | Cuando alguien puede negar que hizo algo                   |
| [[Denning-Sacco]] / timestamps                    | Cuando se agrega timestamp para anti-replay                |
| CIA trade-off (ver [[La triada]])                 | Cuando hay tensión entre disponibilidad e integridad       |
