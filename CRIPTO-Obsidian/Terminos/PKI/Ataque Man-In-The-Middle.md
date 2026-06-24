Ataque activo en el que un adversario se interpone entre dos partes que creen estar comunicándose directamente entre sí. Es una de las principales motivaciones detrás de la [[PKI]].

## Cómo funciona

1. A quiere obtener la clave pública de B
2. El atacante E intercepta la solicitud y responde con **su propia clave pública** en lugar de la de B
3. A cifra mensajes con la clave de E creyendo que es la de B
4. E descifra, lee (y eventualmente reencripta con la clave real de B) y reenvía

Resultado: E puede leer y modificar toda la comunicación sin que A ni B lo noten.

## Condición necesaria

El ataque es posible cuando **no existe un mecanismo confiable para asociar una clave pública con una identidad**. La [[PKI]] y los [[Certificado Digital|certificados digitales]] resuelven exactamente este problema.

## Variante en intercambio de claves simétricas

En protocolos como [[Intercambio Diffie-Hellman]], un MITM puede establecer claves de sesión separadas con cada parte, impersonando a ambas. Sin [[Autenticación]] adicional, Diffie-Hellman es vulnerable a este ataque.

## Contramedidas

- [[Certificado Digital|Certificados digitales]] firmados por una [[Autoridad Certificante]] de confianza
- [[Firma Digital|Firmas digitales]] para autenticar los mensajes del handshake ([[TLS]])
- [[Challenge-Response]] con claves previamente compartidas
- [[Key Distribution Centers|KDC]] en entornos simétricos ([[Protocolo Needham-Schroeder]])

## Ataques relacionados

- **Replay attack**: reutilización de mensajes capturados anteriormente
- **Downgrade attack**: forzar el uso de versiones o algoritmos más débiles (relevante en [[TLS]])
- **Spoofing**: suplantación de identidad en general
