> [!example] Enunciado
> El concejo deliberante de Tecnociudad solicitó un sistema distribuido para referendos que garantice ausencia de votos duplicados y anonimato. Propuesta: cada persona elige un número aleatorio y genera `IDp = H(Rp || D.N.I.)` con `H` libre de colisiones. Al votar, carga `IDp` y el sistema registra `(IDp, valor votado)`. Al terminar, se publica el padrón de `IDp` asociado a cada resultado.
>
> 1. Explicar por qué nadie puede saber el voto de una persona (un DNI) en particular.
> 2. Explicar cómo es posible que una persona efectúe múltiples votaciones.
> 3. Proponer una mejora que evite ese problema sin perder el anonimato.

**Conceptos:**

* [[Funciones de Hash criptograficas]]
* [[Salting]]
* [[Rainbow Table]]

1. Por la no invertibilidad de la función hash. Tampoco se puede buscar por fuerza bruta (un DNI tiene 10^8 posibilidades) porque está concatenado con un número aleatorio que actúa como [[Salting|salt]], protegiendo además contra [[Rainbow Table|rainbow tables]].

2. Una persona puede elegir distintos números aleatorios y publicar distintos `IDp` con su valor votado. Como no se menciona verificación de que el DNI sea válido, incluso podría usar DNIs falsos para manipular los resultados.

3. Es necesaria una fase separada que garantice la integridad del votante sin comprometer el anonimato del voto: (1) la persona se presenta con su DNI, el sistema lo verifica, le entrega un token `T` de un solo uso, marca el DNI como usado, y borra la relación DNI↔T; (2) la persona vota anónimamente con `T`, el sistema verifica que sea válido y no usado, registra el voto, e invalida `T`. Al separar la verificación de identidad (paso 1) del registro del voto (paso 2) y no conservar el vínculo entre ambos, se evita el doble voto sin perder el anonimato.
