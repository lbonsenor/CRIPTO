Lista de [[Certificado Digital|certificados]] que han sido **revocados antes de su fecha de expiración** por la [[Autoridad Certificante]] que los emitió.

## Por qué se necesita

Un certificado puede necesitar invalidarse anticipadamente cuando:
- La [[Criptosistema Asimétrico|clave privada]] asociada fue comprometida
- Cambio de propietario de la clave
- La entidad certificada dejó de existir o cambió sus datos

## Tipos de listas

- **Listado actual**: certificados vigentes que han sido revocados
- **Listado histórico**: certificados ya expirados que fueron revocados (útil para auditoría)

## Reglas en X.509

En [[Certificado X.509|X.509]], **solo el emisor de un certificado puede revocarlo**. La revocación se agrega a la CRL de la [[Autoridad Certificante]] global correspondiente.

## Uso

- La CRL puede **descargarse para validación offline**
- O puede **consultarse online** en tiempo real
- Los certificados [[Certificado X.509|X.509v3]] incluyen el campo *CRL Distribution Points* con la URL de descarga

## Limitaciones

- Las CRLs pueden ser muy grandes en CAs con muchos certificados
- Tienen un período de actualización, por lo que puede haber una ventana donde un certificado revocado aún parezca válido
- Alternativa más eficiente: [[OCSP - Online Certificate Status Protocol]]

## Ver también

- [[OCSP - Online Certificate Status Protocol]]
- [[Autoridad Certificante]]
- [[Certificado X.509]]
- [[PKI]]
- [[Expiración de Claves]]
