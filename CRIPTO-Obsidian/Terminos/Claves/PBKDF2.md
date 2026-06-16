**PBKDF2** (_Password-Based Key Derivation Function 2_, PKCS #5 v2.0) es el método estándar y recomendado para almacenar contraseñas de forma segura.

## Idea central

En lugar de calcular un hash simple de la contraseña, PBKDF2:

1. Calcula un **Salt** aleatorio `S`.
2. Aplica una **función pseudoaleatoria** (PRF, generalmente un [[HMAC]]) iterativamente:
   - `u₁ = PRF(pass, S)`
   - `u₂ = PRF(pass, u₁)`
   - `...`
   - `uⱼ = PRF(pass, uⱼ₋₁)`
3. El resultado es `u₁ ⊕ u₂ ⊕ ... ⊕ uⱼ`.

Cada iteración **aumenta el costo computacional** de cada prueba, haciendo los ataques de fuerza bruta mucho más lentos sin afectar al usuario legítimo de forma significativa.

## Algoritmo formal

```
K = PBKDF2(prf, pass, salt, c, len)

K = T₁ || T₂ || ... || Tₙ       (n = len / |prf|)

Tᵢ = u₁ ⊕ u₂ ⊕ ... ⊕ uᵪ

u₁   = PRF(Pass, Salt || INT32_BE(i))
uₖ   = PRF(Pass, uₖ₋₁)
```

| Parámetro | Descripción |
|---|---|
| `prf` | Función pseudoaleatoria (ej: HMAC-SHA1) |
| `pass` | Contraseña del usuario |
| `salt` | Valor aleatorio único por usuario |
| `c` | Número de iteraciones |
| `len` | Longitud del resultado deseado |

## Implementación en Java (JCE)

```java
PBEKeySpec spec = new PBEKeySpec(password, salt, iterations, bytes * 8);
SecretKeyFactory skf = SecretKeyFactory.getInstance("PBKDF2WithHmacSHA1");
byte[] key = skf.generateSecret(spec).getEncoded();
```

## Por qué es seguro

- Incorpora [[Salting]]: evita ataques en paralelo entre cuentas.
- Las **iteraciones** (`c`) incrementan el costo de cada prueba del atacante. Con `c = 100.000`, el atacante necesita 100.000 veces más tiempo por intento.
- El número de iteraciones puede aumentarse conforme el hardware se vuelve más rápido.

## Relación con otros conceptos

- [[Salting]]: componente integrado en PBKDF2.
- [[HMAC]]: la PRF usada internamente.
- [[Claves]]: el activo que protege.
- [[Ataques a Sistemas de Autenticación]]: el ataque que mitiga.
- [[Fórmula de Anderson]]: PBKDF2 reduce efectivamente el `G` del atacante.
