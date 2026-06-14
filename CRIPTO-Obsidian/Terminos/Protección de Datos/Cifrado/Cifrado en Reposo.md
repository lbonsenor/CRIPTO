# Cifrado en Reposo

El **cifrado en reposo** (_encryption at-rest_) protege datos almacenados en cualquier medio físico o virtual.

Ver [[Estados de los Datos]] para el contexto general.

## Implementaciones comunes

| Sistema | Descripción |
|---|---|
| **EncFS / Loop-AES** | Cifrado de sistemas de archivos en Linux |
| **EFS** | Encrypting File System de Windows |
| **FileVault** | Cifrado de disco completo en macOS |
| **[[KEK-DEK]]** | Esquema de doble clave para gestionar el cifrado de grandes volúmenes de datos |
| **[[HSM]]** | Hardware Security Module — dispositivo físico especializado en operaciones criptográficas |

## Consideraciones

- **Data Remanence**: los datos pueden persistir en medios de almacenamiento incluso después de ser "eliminados". El cifrado mitiga este riesgo.
- **Secure Deletion**: eliminación que sobrescribe los datos para evitar su recuperación.

## Cifrado en reposo en la nube

La nube introduce opciones y trade-offs entre **control** y **costo operativo**:

| Opción | Quién cifra | Quién gestiona claves | Control | Costo operativo |
|---|---|---|---|---|
| **Client-Side Encryption** | El cliente, antes de subir | El cliente (tiene las claves privadas y decide la rotación) | Máximo | Alto |
| **BYOK** (_Bring Your Own Key_) | El proveedor | El cliente (puede rotar, pero no accede a las claves privadas directamente) | Alto | Medio |
| **Managed Keys** | El proveedor | El proveedor (decide rotación; cliente puede usarlas para lo que el proveedor permita) | Medio | Bajo |
| **Provider-Managed** | El proveedor | El proveedor (decisión y rotación completamente del proveedor) | Mínimo | Mínimo |

> Delegar el cifrado al proveedor cloud significa **delegar el control de las claves** a un tercero. Si el proveedor es comprometido o actúa de mala fe, los datos quedan expuestos.

## Relación con otros conceptos

- [[Estados de los Datos]]: el estado "at-rest" que protege.
- [[KEK-DEK]]: esquema de gestión de claves para datos at-rest.
- [[HSM]]: dispositivo para gestión segura de claves.
- [[Criptosistema]]: los algoritmos de cifrado que se usan.
- [[CSPM]]: verifica que los recursos cloud estén correctamente cifrados.

## Referencia

https://www.datacenterknowledge.com/cloud/data-at-rest-encryption-in-the-cloud-explore-your-options
