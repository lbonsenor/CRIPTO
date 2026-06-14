# Homomorphic Encryption

El **cifrado homomórfico** es un esquema de cifrado que permite realizar **operaciones sobre datos cifrados** sin necesidad de descifrarlos, obteniendo un resultado que, al descifrar, es equivalente a haber operado sobre los datos en claro.

## Propiedad formal

$$f(x) = y \implies f(\text{Enc}(x)) = \text{Enc}(y)$$

Es decir: aplicar la función sobre el dato cifrado produce el **cifrado del resultado**, sin revelar el dato original.

## Implicaciones prácticas

Permite delegar el **procesamiento de datos sensibles** a un tercero (ej: un proveedor cloud) sin revelarle los datos:

```
Cliente ──► Enc(datos) ──►  Servidor cloud
                                 │
                           f(Enc(datos)) = Enc(resultado)
                                 │
Cliente ◄── Dec(resultado) ◄──────┘
```

El servidor procesa los datos **sin verlos nunca en claro**.

## Casos de uso

- Procesamiento de datos médicos o financieros en la nube sin exponer los datos al proveedor.
- Búsquedas sobre datos cifrados.
- Machine learning sobre datos sensibles.

## Estado actual

El cifrado homomórfico **existe en implementaciones prácticas**, pero tiene un costo computacional muy alto — órdenes de magnitud más lento que el procesamiento sobre datos en claro. Es un área activa de investigación.

## Relación con otros conceptos

- [[Cifrado en Reposo]]: el cifrado homomórfico extiende la protección al momento del procesamiento.
- [[Estados de los Datos]]: aborda el estado "in-use" de forma novedosa.
- [[Cloud Vulnerabilities]]: el homomórfico elimina la necesidad de confiar en el proveedor cloud para el procesamiento de datos sensibles.
- [[Secreto Perfecto]]: la privacidad que el homomórfico busca preservar durante el procesamiento.

## Referencia

_Protecting privacy through homomorphic encryption_, Lauter 2022
