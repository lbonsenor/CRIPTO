# Cifrado en Memoria

El **cifrado en memoria** protege datos que están siendo procesados activamente en RAM, mitigando el riesgo de acceso desde otros procesos o máquinas virtuales que comparten el mismo hardware físico.

Ver [[Estados de los Datos]] para el contexto general.

## El problema

En entornos de múltiples VMs o contenedores compartiendo el mismo hardware, existe el riesgo potencial de que un proceso acceda a regiones de memoria usadas por otro. Esto es especialmente relevante en:

- Ambientes cloud con múltiples tenants.
- Hypervisores comprometidos.
- Ataques de tipo [[Side-Channel Attack]] basados en memoria compartida.

## Solución: TME (Total Memory Encryption)

Intel implementó **TME** (_Total Memory Encryption_) como mecanismo de hardware para cifrar la memoria del sistema.

### TME básico

- **Una única clave de cifrado** compartida por todos los procesos del sistema.
- El cifrado/descifrado se realiza **dentro de la CPU**, antes de que los datos lleguen a la caché.
- Protege contra ataques físicos al bus de memoria (cold boot, DMA attacks).
- No diferencia entre procesos — todos usan la misma clave.

### TME-MK (Multi-Key)

- **Múltiples claves de cifrado**, una por VM o por región de memoria.
- El control de acceso a cada clave está gestionado por los **registros VT-x** del procesador.
- Permite que cada VM tenga su propia clave → aislamiento criptográfico entre tenants.
- Incluso el hypervisor no puede leer la memoria de una VM sin su clave.

```
VM1  ──►  Clave K1  ──►  Región de memoria cifrada M1
VM2  ──►  Clave K2  ──►  Región de memoria cifrada M2
```

## Relación con otros conceptos

- [[Estados de los Datos]]: el estado "in-use" que protege.
- [[Virtualización]]: el contexto donde TME-MK tiene mayor relevancia.
- [[Side-Channel Attack]]: una de las clases de ataque que TME mitiga.
- [[Contenedores]]: TME-MK también aplica a ambientes de contenedores multi-tenant.

## Referencia

https://www.intel.com/content/www/us/en/developer/articles/news/runtime-encryption-of-memory-with-intel-tme-mk.html
