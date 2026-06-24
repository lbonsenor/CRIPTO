Protocolo online que permite verificar si un [[Certificado Digital]] está **activo o revocado**, como alternativa más eficiente a las [[CRL - Certificate Revocation List|listas CRL]].

## Características

- Servicio online con API estandarizada
- Administrado por las [[Autoridad Certificante|Autoridades Certificantes]]
- El resultado (respuesta OCSP) está **firmado digitalmente**, lo que lo convierte en un certificado de corta duración
- Duración típica: de 1 hora a 7 días

## OCSP Stapling

Variante en la que el **servidor incluye la respuesta OCSP** dentro de la cadena de certificados durante el [[TLS|handshake TLS]], evitando que el cliente deba consultar una fuente externa.

**Ventajas del stapling:**
- Reduce la latencia (no hay una consulta adicional del cliente)
- Mejora la privacidad (la CA no sabe qué sitios visita el usuario)
- Recomendado para sitios de alto tráfico

## Comparación con CRL

| | CRL | OCSP |
|--|-----|------|
| Tipo | Lista completa | Consulta individual |
| Tamaño | Puede ser grande | Respuesta pequeña |
| Latencia | Descarga periódica | Consulta en tiempo real |
| Privacidad | Mejor | CA sabe qué cert. se consulta |
| Disponibilidad | Offline posible | Requiere conectividad (sin stapling) |

## Ver también

- [[CRL - Certificate Revocation List]]
- [[Certificado Digital]]
- [[Autoridad Certificante]]
- [[TLS]]
- [[PKI]]
