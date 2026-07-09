> [!example] Enunciado
> El consejo deliberante de Tecnociudad solicitó un sistema distribuido para licitaciones transparentes y seguras. Durante la fase de ofertas, los costos son secretos; al vencer el plazo se publican precio y propuesta y se elige el de menor costo cumpliendo condiciones.
>
> Implementación: por cada licitación se reciben archivos cifrados con **AES-GCM** de los oferentes (documento con ID de licitación, descripción técnica, CUIT y precio). Al terminar el plazo, los oferentes comparten la clave usada, se revisan las ofertas y se adjudica. Si un oferente no provee la clave, queda descalificado. Se publican todas las ofertas descifradas y la adjudicada en un portal público.
>
> 1. Explicar por qué ningún oferente puede alterar su propuesta sin ser detectado.
> 2. ¿Qué medidas adicionales evitarían que un administrador ignore a un proveedor diciendo que nunca entregó la clave?
> 3. Un oferente envía un archivo a nombre de un competidor con precio irrisoriamente bajo para perjudicarlo. ¿Cómo evitar esto con funciones criptográficas?

**Conceptos:**

* [[GCM - Galois Counter Mode]]
* [[Firma Digital]]
* [[Modelo Clark-Wilson]]

1. [[GCM - Galois Counter Mode|AES-GCM]] provee confidencialidad e integridad: genera un tag de autenticación que depende de la clave, el nonce y el contenido completo. Si el oferente modifica cualquier parte del archivo (precio, descripción, CUIT), al descifrar con la clave original la verificación del tag falla y la alteración es detectada.

2. **Acuse de recibo firmado (no repudio de recepción):** al entregar la clave, el oferente la firma: `(clave, id_licitacion, id_oferente, timestamp, Sign_sk_oferente(...))`; el administrador firma un acuse de recibo con su propia sk. Así ninguna de las dos partes puede negar su participación — se usa [[Firma Digital]] y no MAC porque MAC requeriría clave compartida y cualquiera de las partes podría haberlo generado. Además, **logs de auditoría** ([[Modelo Clark-Wilson]] C4) accesibles a un auditor independiente, y publicación de las entregas y acuses en el portal público para transparencia.

3. Cada oferente tiene un par (sk, pk) registrado al inscribirse. Antes de cifrar, firma su documento: `firma = Sign_sk_oferente(H(doc))`, y cifra `doc || firma` con AES-GCM. Al abrir la licitación, el sistema descifra, busca la pk asociada al CUIT del documento, y verifica `Vrfy_pk_oferente(H(doc), firma)`; si falla, la oferta se rechaza. Esto garantiza **no repudio**: solo el poseedor de la sk puede generar firmas válidas.
