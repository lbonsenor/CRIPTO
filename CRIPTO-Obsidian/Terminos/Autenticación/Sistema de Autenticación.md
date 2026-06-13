Modelo formal de un sistema de [[Autenticación]]. Define los componentes que intervienen en la verificación de identidad.

## Componentes

| Símbolo | Nombre | Descripción |
|---|---|---|
| `A = { a }` | **Información de autenticación** | Designación de la entidad e información provista por ella |
| `C = { c }` | **Información complementaria** | Especificación del principal e información almacenada por el sistema |
| `F = { F: A → C }` | **Funciones de complementación** | Derivan información complementaria a partir de A |
| `L = { L: A×C → {0,1} }` | **Funciones de autenticación** | Determinan si un par (A, C) es una asociación válida |
| `S = { s }` | **Funciones de selección** | Permiten crear y actualizar A y C |

## Ejemplo: autenticación con claves

```
A = { x / x es clave }
C = h(A)
F = { h(x) }
L = { == }     ← F(A) = C es válido
S = { adduser(), removeuser(), passwd() }
```

## Ejemplo: sistema Unix legacy

```
A = { secuencias de hasta 8 caracteres }
C = { xxHHHHHHHHHHH }
    xx     = función de hash (0–4095) → 2 caracteres
    HH...H = resultado de la función   → 11 caracteres
F = { DES(.) }
L = { login, su, sudo, ... }
S = { passwd, adduser, ... }
```

Se almacena por cada usuario el resultado de una de 4096 funciones de hash.

## Ataques al sistema

Ver: [[Ataques a Sistemas de Autenticación]]

## Relación con otros conceptos

- [[Autenticación]]: el proceso que este modelo formaliza.
- [[Claves]]: el tipo más común de información de autenticación.
- [[PBKDF2]]: método correcto para derivar C a partir de A.
- [[Salting]]: técnica para reforzar la función F.
