## Objetivo del atacante

Ser identificado como una entidad legítima (**impersonarla**).

### Mecanismo general

Encontrar un `a ∈ A` tal que, para algún `f ∈ F`:

```
f(a) = c
```

donde `c` está asociado a una entidad válida.

## Tipos de ataques

### Ataques offline

El atacante ya posee `c` (ej: robó el archivo de contraseñas) y prueba candidatos localmente:

- Probar varios `a`, computar `f(a)` y comparar con `c`.
- No hay límite de intentos, puede hacerse en paralelo con hardware especializado (GPUs).

**Ejemplos:**
- Obtener `/etc/shadow` en Linux y atacarlo con `crack` o `john the ripper`.
- Obtener el archivo `SAM` de Windows y atacarlo con `ophcrack`.

### Ataques online

Usar la función `L` directamente, probando con diferentes `a` hasta pasar la autenticación.

**Ejemplos:**
- Probar la función de login con cuentas conocidas (`root`, `administrator`, `guest`).

## Técnicas de ataque por adivinanza

Búsqueda aleatoria pura es ineficiente. Las mejoras incluyen:

- **Diccionarios** de palabras comunes.
- **Diccionarios de claves descubiertas** en brechas previas.
- **Transformaciones simples**: sufijos, prefijos, `l→1`, `o→0`, vocales por números, palabras espejadas, combinaciones de dos palabras.
- **Información de contexto**: nombre, usuario, DNI, fecha de nacimiento.

## Estimación de tiempo de ataque

Ver: [[Fórmula de Anderson]]

## Prevenciones

- **Esconder** `a`, `c` o `f` para que no sean conocidas simultáneamente (ej: `/etc/shadow` en Unix).
- **Limitar** el uso de la función de autenticación:
  - Tiempos crecientes ante fallas.
  - Deshabilitar principals tras N intentos.
  - Jailing / Honeypot.
  - CAPTCHAs.
- **Aumentar la complejidad** de las claves → ver [[Fórmula de Anderson]].
- **Salting** → ver [[Salting]].
- **PBKDF2** → ver [[PBKDF2]].

## Relación con otros conceptos

- [[Sistema de Autenticación]]: el modelo formal que se ataca.
- [[Claves]]: el activo más frecuentemente atacado.
- [[Salting]]: defensa contra ataques offline en paralelo.
- [[PBKDF2]]: defensa que aumenta el costo computacional del ataque.
- [[Fórmula de Anderson]]: herramienta para estimar riesgo.
