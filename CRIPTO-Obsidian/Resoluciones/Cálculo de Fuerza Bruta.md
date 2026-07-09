Fórmula base: `Tiempo total = (tamaño del espacio de claves) × (tiempo por intento)`.

Tamaños de espacio típicos:
```
PIN de 4 dígitos:      10^4  = 10.000 combinaciones
PIN de 8 dígitos:      10^8  = 100.000.000 combinaciones
Contraseña 8 caracteres alfanuméricos: ~62^8 ≈ 2 × 10^14 combinaciones
```

Comparación de velocidad de hash sobre un PIN de 8 dígitos:
```
Con SHA-256 (~1 μs por intento):
  10^8 × 1 μs = 100 segundos  ← INSEGURO para un PIN

Con bcrypt (~100 ms por intento):
  10^8 × 100 ms = 10^7 s ≈ 115 días  ← SEGURO para un PIN
```

Esta cuenta es la respuesta estándar cuando el final pregunta "¿por qué no usar SHA-256 para un PIN?": ver [[Fuerza Bruta]] y [[Bcrypt]].
