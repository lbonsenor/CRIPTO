# HSM — Hardware Security Module

Un **HSM** (_Hardware Security Module_) es un dispositivo físico especializado en la **generación, almacenamiento y gestión segura de claves criptográficas** y en la ejecución de operaciones criptográficas.

## Función

- Almacena claves criptográficas en hardware **a prueba de tampering**: si alguien intenta abrir o manipular el dispositivo físicamente, las claves se destruyen automáticamente.
- Ejecuta operaciones criptográficas (firma, cifrado, descifrado) **dentro del dispositivo** — las claves nunca salen del HSM en texto plano.
- Provee un entorno de confianza para operaciones críticas que no puede ser comprometido desde software.

## Estándar FIPS

Los HSMs se certifican bajo el estándar **FIPS 140** (_Federal Information Processing Standard_), con cuatro niveles de seguridad:

| Nivel | Descripción |
|---|---|
| **FIPS 140-1** | Seguridad básica en software |
| **FIPS 140-2** | Seguridad física básica — el más común en la industria |
| **FIPS 140-3** | Protección física avanzada, detección de tampering |
| **FIPS 140-4** | Máxima seguridad, resistencia a ataques físicos sofisticados |

## Casos de uso

- Almacenamiento de la **KEK** en un esquema [[KEK-DEK]].
- Generación y almacenamiento de claves privadas para [[Criptosistema Asimétrico|criptosistemas asimétricos]].
- Operaciones de [[Firma Digital]] en infraestructuras de alta seguridad.
- Raíces de confianza en PKI (_Public Key Infrastructure_).

## Relación con otros conceptos

- [[Cifrado en Reposo]]: el HSM es el componente que protege las claves maestras.
- [[KEK-DEK]]: la KEK es el activo típicamente almacenado en un HSM.
- [[Firma Digital]]: el HSM garantiza que las claves privadas de firma no puedan ser extraídas.
- [[Side-Channel Attack]]: los HSMs están diseñados para resistir este tipo de ataques físicos.
