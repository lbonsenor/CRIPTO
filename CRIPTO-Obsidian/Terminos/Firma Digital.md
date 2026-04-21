Es una terna de algoritmos:
$\text{Gen}:()\to PK\times SK$
$\text{Sign}:SK\times P\to S$
$\text{Vrfy}:PK\times P\times S\to\{0,1\}$

Tal que:
- PK es el conjunto de todas las claves públicas posibles
- SK es el conjunto de todas las claves privadas posibles
- P, espacio de texto plano, es el conjunto de todos los mensajes posibles
- S, espacio de firmas, es el conjunto de todas las firmas posibles

Son similares a los [[Message Authentication Code|MACs]], pero son:
- Públicamente verificables
- Transferibles
- Proveen no repudio (quien firma no puede negar haberlo hecho)

> [!info] Propiedad básica
> $$\forall m,(pk,sk):\text{Vrfy}_{pk}(m,\text{Sign}_{sk}(m))=1$$