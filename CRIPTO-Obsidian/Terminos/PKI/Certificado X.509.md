Estándar más ampliamente adoptado para [[Certificado Digital|certificados digitales]]. Definido por la ITU-T, forma la base de la [[PKI]] moderna.

## Campos del certificado

| Campo | Descripción |
|-------|-------------|
| Version | Versión del estándar (actualmente v3) |
| Serial Number | Número único asignado por la [[Autoridad Certificante]] |
| Signature Algorithm | Algoritmo usado para firmar (ej: `SHA256WithRSAEncryption`) |
| Issuer | Nombre distinguido (DN) de la CA emisora |
| Validity | Intervalo de validez (`Not Before` / `Not After`) |
| Subject | Nombre distinguido del sujeto (incluye CN) |
| Subject Public Key Info | Clave pública del sujeto + algoritmo |
| Extensions (v3) | Uso de clave, restricciones básicas, SAN, etc. |
| Signature | [[Firma Digital]] del hash de todos los campos anteriores |

## Extensiones X.509v3 relevantes

- **Basic Constraints**: indica si el certificado pertenece a una CA (`CA:TRUE`)
- **Key Usage**: cifrado, firma, firma de certificados, etc.
- **Subject Alternative Name (SAN)**: nombres adicionales del sujeto (wildcard, múltiples dominios)
- **CRL Distribution Points**: dónde obtener la [[CRL - Certificate Revocation List]]
- **Authority Information Access**: URL del servicio [[OCSP - Online Certificate Status Protocol]]

## Cadenas de certificados

Los certificados X.509 forman [[Cadena de Confianza|cadenas de confianza]]: cada certificado está firmado por una [[Autoridad Certificante]], cuyo propio certificado está firmado por otra CA, hasta llegar a una [[CA Raíz]].

## Proceso de verificación

1. Obtener la clave pública del emisor (de la cadena o del sistema si es raíz)
2. Verificar recursivamente el certificado del emisor si es necesario
3. Validar la [[Firma Digital]] usando la clave pública del emisor
4. Comprobar el intervalo de validez
5. Verificar identidad (CN o SAN)
6. Verificar el uso permitido del certificado
7. Consultar revocación vía [[CRL - Certificate Revocation List]] u [[OCSP - Online Certificate Status Protocol]]

## Ver también

- [[PKI]]
- [[Autoridad Certificante]]
- [[Cadena de Confianza]]
- [[TLS]]
