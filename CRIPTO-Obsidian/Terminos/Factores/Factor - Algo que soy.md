El factor **"algo que soy"** basa la identificación en **características intrínsecas** de la entidad (_biometría_).

## Ejemplos

- Huella digital
- Retina / iris
- Voz
- Cara (reconocimiento facial)

## Características

- Las características biométricas **no son fácilmente modificables**.
- Elimina el problema de olvidar una clave o perder un token.
- Los métodos actuales **no son 100% eficaces**: existen tasas de falsos positivos (FAR) y falsos negativos (FRR).

## Supuestos y riesgos

- Asume que el **dispositivo de lectura no puede ser manipulado** (spoofing de sensor).
- Una característica biométrica comprometida **no puede cambiarse** (a diferencia de una contraseña).
- Ataques de _presentation attack_ (fotos, huellas artificiales, deepfakes de voz/cara).

## Métricas de evaluación

| Métrica | Descripción |
|---|---|
| **FAR** (_False Acceptance Rate_) | % de impostores aceptados incorrectamente |
| **FRR** (_False Rejection Rate_) | % de usuarios legítimos rechazados incorrectamente |
| **EER** (_Equal Error Rate_) | Punto donde FAR = FRR; indica la precisión del sistema |

## Relación con otros conceptos

- [[Factor de Autenticación]]: categoría a la que pertenece.
- [[Multi-Factor Authentication]]: se combina con otros factores para mitigar sus limitaciones.
