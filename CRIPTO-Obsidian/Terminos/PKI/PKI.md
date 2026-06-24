Infraestructura de clave pública (**Public Key Infrastructure**). Su objetivo es **asociar una identidad a una [[Criptosistema Asimétrico|clave pública]]**, evitando problemas de suplantación como el [[Ataque Man-In-The-Middle]].

No es aplicable a [[Criptosistema|criptosistemas simétricos]], ya que en esos casos la selección de claves depende exclusivamente de las partes involucradas. Usar la clave equivocada significa que no hay garantías de [[Seguridad|confidencialidad]] ni integridad.

## Problema que resuelve

Cuando A quiere comunicarse con B usando [[Criptosistema Asimétrico|criptografía asimétrica]], necesita obtener la clave pública de B. Si esa asociación clave-identidad es vulnerable, un atacante puede realizar un [[Ataque Man-In-The-Middle]] substituyendo la clave pública.

## Componentes

- [[Certificado Digital]]: asocia una clave pública con una identidad, firmado por una [[Autoridad Certificante]]
- [[Autoridad Certificante]] (CA): entidad que emite y firma certificados
- [[Cadena de Confianza]]: jerarquía de CAs que permite validar certificados
- [[CRL - Certificate Revocation List]]: lista de certificados revocados
- [[OCSP - Online Certificate Status Protocol]]: protocolo online para verificar estado de certificados

## Usos comunes

- Comunicaciones HTTPS ([[TLS]])
- Firma de correos electrónicos
- Firma de código y documentos
- [[Firma Digital|Firmas digitales]] en general

## Ver también

- [[Intercambio de Claves]]
- [[Ataque Man-In-The-Middle]]
- [[Certificado X.509]]
