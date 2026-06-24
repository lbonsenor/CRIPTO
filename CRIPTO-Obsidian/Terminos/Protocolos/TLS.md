Protocolo estándar para comunicaciones seguras sobre redes inseguras. Provee **confidencialidad, integridad y autenticación** a nivel de transporte. Evolución de [[SSL]].

## Historia

- **SSL** (Secure Socket Layer): versión inicial, creada por Netscape
- **TLS 1.0**: estandarización de SSL 3.0 (1999)
- **TLS 1.2**: mejoras significativas (2008)
- **TLS 1.3**: versión actual, más segura y rápida (RFC 8446, 2018)

TLS 1.2 equivale aproximadamente a SSL 3.3.

## Tipos de mensajes TLS

| Tipo | Descripción |
|------|-------------|
| **Handshake** | Configuración inicial, autenticación y negociación criptográfica |
| **Application Data** | Datos del protocolo encapsulado (ej: HTTP) |
| **Alert** | Eventos y errores |
| **Change Cipher Spec** | Indica cambio/renegociación de claves de sesión |

## TLS Record

Capa base del protocolo. Para cada mensaje:
1. Divide en bloques de hasta 2¹⁶ bytes
2. Comprime cada bloque y calcula su hash
3. Cifra el bloque + hash
4. Agrega un header y lo envía al transporte subyacente (ej: TCP)

## Opciones criptográficas

**Intercambio de claves:**
- RSA
- [[Intercambio Diffie-Hellman]] (con cert. RSA o DSS)
- Diffie-Hellman anónimo
- ECDH (Diffie-Hellman sobre curvas elípticas)
- [[Kerberos]]

**Cifrado simétrico:**
- [[AES]]-CBC, AES-CCM, AES-GCM
- ChaCha20 (TLS 1.2+)
- [[3-DES|Triple DES]] (obsoleto)
- RC4 (obsoleto)

**[[Message Authentication Code|MAC]]:**
- [[HMAC]]-SHA1
- [[HMAC]]-SHA2 (256/384)
- AEAD (Authenticated Encryption with Associated Data)

**[[Autenticación]]:**
- [[Firma Digital|Firmas RSA]]
- [[Digital Signature Standard|Firmas DSS]]

## Sesión TLS vs Conexión TLS

- **Sesión**: asociación entre dos pares. Puede soportar múltiples conexiones. Contiene el *Master Secret* (clave compartida de 48 bytes).
- **Conexión**: describe cómo intercambiar datos. Contiene bits aleatorios, claves de escritura, claves para MACs, IVs y números de secuencia.

## TLS Handshake

Proceso de 4 partes:

**Parte 1 – Negociación:**
- `ClientHello`: versión, nonce `r1`, session ID, lista de cipher suites y métodos de compresión
- `ServerHello`: versión acordada, nonce `r2`, session ID, cipher suite y compresión elegidos

**Parte 2 – Autenticación del servidor:**
- `Certificate`: cadena de certificados [[Certificado X.509|X.509]] del servidor
- `ServerKeyExchange`: parámetros criptográficos firmados con la clave privada del servidor
- `CertificateRequest` (opcional): solicita certificado al cliente
- `ServerHelloDone`

**Parte 3 – Intercambio de clave:**
- `ClientCertificate` (si fue solicitado)
- `ClientKeyExchange` (RSA): envía `{ V || PRE_rand }Ks` — el pre-master secret cifrado con la clave pública del servidor
- `ClientKeyExchange` (DH): envía `g^b mod p`

Los nonces `r1` y `r2` evitan replay attacks. La versión incluida en el ClientKeyExchange previene **downgrade attacks**.

**Parte 4 – Confirmación:**
- `ChangeCipherSpec`: indica que a partir de aquí se usan los parámetros negociados
- `Finished`: hash de todos los mensajes intercambiados, protegido con el master secret

El servidor responde con su propio `ChangeCipherSpec` + `Finished`.

## TLS Alert

- Puede ser de **advertencia** o **fatal**
- Un evento fatal **invalida la conexión**
- `CloseNotify`: indica el fin de una sesión

**Errores fatales comunes:**
- Mensaje no esperado
- MAC incorrecto
- Error en el handshake
- Parámetro ilegal

**Advertencias comunes:**
- Certificado no válido, expirado o revocado
- Certificado no soportado

## Dependencias

- Depende de [[PKI]] para autenticación del servidor (y opcionalmente del cliente)
- Usa [[Certificado X.509]] para los certificados
- La autenticación del cliente es opcional y poco frecuente en la práctica

## Ver también

- [[SSL]]
- [[PKI]]
- [[Certificado X.509]]
- [[Intercambio Diffie-Hellman]]
- [[HMAC]]
- [[AES]]
- [[Firma Digital]]
