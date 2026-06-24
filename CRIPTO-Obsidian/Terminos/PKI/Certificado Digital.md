Mensaje firmado digitalmente que asocia una [[Criptosistema Asimétrico|clave pública]] con una identidad. Son el mecanismo central de la [[PKI]].

## Contenido mínimo

- **Información de identidad**: nombre del sujeto (CN - Common Name)
- **Clave pública** asociada al sujeto
- **Fecha de emisión**
- **Intervalo de validez** (not before / not after)
- **Tipo de uso** permitido de la clave:
  - Firma de mensajes
  - Cifrado de emails
  - Firma de certificados
  - Cifrado de sitios web (TLS)
- **[[Firma Digital]]** del emisor ([[Autoridad Certificante]])

## Common Name (CN) según uso

| Uso | CN |
|-----|----|
| Email | dirección de email |
| Servidor | IP o hostname |
| Empresa | razón social |

## Validación de un certificado

Para validar un certificado, se deben verificar:

1. **Integridad**: validar la [[Firma Digital]] de la [[Autoridad Certificante]] emisora
2. **Vigencia**: comprobar que la fecha actual está dentro del intervalo de validez
3. **Identidad**: comparar el CN con el esperado por la aplicación
4. **Uso**: verificar que el tipo de uso del certificado coincide con el uso que se le quiere dar
5. **Revocación**: consultar la [[CRL - Certificate Revocation List]] o [[OCSP - Online Certificate Status Protocol]]

## Estándar

El formato más utilizado es [[Certificado X.509]].

## Ver también

- [[Autoridad Certificante]]
- [[Cadena de Confianza]]
- [[CRL - Certificate Revocation List]]
- [[OCSP - Online Certificate Status Protocol]]
- [[PKI]]
