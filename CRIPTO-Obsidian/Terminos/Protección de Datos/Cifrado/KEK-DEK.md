El esquema **KEK-DEK** (_Key Encryption Key / Data Encryption Key_) es un patrón de gestión de claves utilizado para cifrar grandes volúmenes de datos de forma eficiente y segura.

## Problema que resuelve

Cifrar todos los datos directamente con una clave maestra presenta problemas:

- Si la clave maestra se rota, **todos los datos deben volver a cifrarse**.
- Si la clave maestra se compromete, **todos los datos quedan expuestos**.

## Solución: doble nivel de claves

```
Datos  ──── cifrados con ────►  DEK (Data Encryption Key)
DEK    ──── cifrado con  ────►  KEK (Key Encryption Key)
```

| Clave | Función |
|---|---|
| **DEK** | Cifra los datos directamente. Es única por archivo/registro/volumen. Es efímera y puede cambiarse frecuentemente. |
| **KEK** | Cifra la DEK. Es la clave que realmente se protege (idealmente en un [[HSM]]). |

## Ventajas

- **Rotación eficiente**: rotar la KEK solo requiere re-cifrar las DEKs (pequeñas), no todos los datos.
- **Revocación granular**: comprometer una DEK solo expone los datos cifrados con esa DEK, no todos los datos.
- **Separación de responsabilidades**: quien gestiona la KEK y quien gestiona los datos pueden ser distintos.

## Uso en la práctica

Es el esquema detrás de la gestión de claves en servicios como AWS KMS, Azure Key Vault y GCP Cloud KMS, donde el servicio gestiona la KEK y genera DEKs para cada recurso.

## Relación con otros conceptos

- [[Cifrado en Reposo]]: el contexto donde se aplica.
- [[HSM]]: donde se almacena y protege la KEK.
- [[Intercambio de Claves]]: el KEK-DEK es un patrón análogo a los esquemas de intercambio seguro de claves simétricas.
- [[Criptosistema Asimétrico]]: en algunas implementaciones, la KEK es una clave asimétrica.
