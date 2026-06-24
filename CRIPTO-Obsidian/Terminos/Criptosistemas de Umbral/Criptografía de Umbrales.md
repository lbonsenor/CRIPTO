Rama de la criptografía que permite distribuir una operación o un secreto entre **N partes**, requiriendo que al menos **T** de ellas cooperen para reconstruirlo o ejecutar la operación. Formalmente conocida como **(t,n)-threshold cryptography**.

## Definición formal

Es una terna de algoritmos:

- `Gen: () → K^N` — Generador de claves (produce N sombras)
- `Enc: K^T × P → C` — Cifrado con T claves cualesquiera
- `Dec: K^T × C → P` — Descifrado con T claves cualesquiera

**Propiedad básica:** para todo mensaje `m` y claves válidas, cualquier subconjunto de T sombras puede descifrar lo que fue cifrado con cualquier otro subconjunto de T sombras.

## Conceptos clave

| Símbolo | Significado |
|---------|-------------|
| N | Cantidad total de sombras disponibles |
| T | Umbral: cantidad mínima de sombras necesarias |
| K^X | X claves, cada una perteneciente al espacio K |
| {K_x}^n | Subconjunto de n claves cualesquiera de K^X |

## Aplicaciones

- **Custodia de claves maestras**: dividir la clave raíz de un [[HSM]] entre múltiples custodios
- **Firma distribuida**: requerir múltiples firmas para aprobar una transacción
- **Recuperación ante desastres**: no hay un único punto de fallo para material criptográfico sensible
- **[[Secreto Compartido]]**: variante donde el secreto en sí es lo que se distribuye

## Ver también

- [[Método de Shamir]]
- [[Secreto Compartido]]
- [[Claves]]
- [[HSM]]
