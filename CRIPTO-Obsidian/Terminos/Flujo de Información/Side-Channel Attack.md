Un **ataque de canal lateral** (_side-channel attack_) explota las **emisiones físicas y temporales** de un sistema para extraer información sensible, sin basarse en vulnerabilidades clásicas del software ni errores de autenticación.

## Principio

En lugar de atacar el algoritmo matemático directamente, el atacante **observa efectos colaterales del hardware** durante su funcionamiento para inferir información interna (típicamente claves criptográficas).

## Vectores de ataque

| Vector | Técnica | Referencia |
|---|---|---|
| **Tiempo de ejecución** | Medir el tiempo que toma una operación criptográfica para inferir la clave | _Timing Attacks on Implementations of DH, RSA, DSS_ |
| **Consumo de energía** | Analizar el consumo eléctrico de un dispositivo durante operaciones criptográficas (_Power Analysis_) | _Introduction to Differential Power Analysis_ |
| **Radiación electromagnética** | Capturar emisiones EM para obtener información sobre la operación interna y sus claves | _Investigations of Power Analysis Attacks on Smartcards_ |
| **Ruido acústico** | Analizar los sonidos emitidos por componentes del hardware durante el procesamiento | — |

## Relevancia en criptografía

Es especialmente crítico en implementaciones de [[Criptosistema Asimétrico|criptografía asimétrica]] (RSA, ECC) y [[Message Authentication Code|MACs]], donde las diferencias de timing o energía pueden correlacionarse con bits de la clave privada.

## Diferencia con Covert Channel

| | [[Covert Channel]] | Side-Channel Attack |
|---|---|---|
| Objetivo | Transmitir información oculta entre procesos | Extraer información (clave, secreto) de un sistema |
| Agente | Dos partes coordinadas | Un atacante externo observando |
| Contramedida típica | Aislamiento, confinamiento | Implementaciones _constant-time_, ruido aleatorio |

## Relación con otros conceptos

- [[Covert Channel]]: concepto relacionado con distinto modelo de amenaza.
- [[Problema del Confinamiento]]: ambos ataques son consecuencia del confinamiento imperfecto.
- [[Secreto Compartido]]: el activo típicamente comprometido por estos ataques.
