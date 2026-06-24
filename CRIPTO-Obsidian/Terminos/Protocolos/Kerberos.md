Protocolo de [[Autenticación]] de red basado en [[Key Distribution Centers|KDC]], derivado del [[Protocolo Needham-Schroeder]]. Es el protocolo de autenticación estándar en entornos Windows (Active Directory) y muchos sistemas Unix.

## Principios

- Utiliza [[Criptosistema|criptografía simétrica]]
- Se basa en un tercero de confianza centralizado (KDC)
- Usa **timestamps** para prevenir replay attacks (modificación Denning-Sacco)
- El usuario se autentica una vez y obtiene tickets para acceder a múltiples servicios ([[SSO - Single Sign-On]])

## Componentes del KDC

- **AS** (Authentication Server): autentica al usuario y emite el TGT
- **TGS** (Ticket Granting Server): emite tickets de servicio a partir del TGT

## Flujo simplificado

```
Usuario → AS: solicitud de autenticación
AS → Usuario: TGT (Ticket Granting Ticket) cifrado con clave del usuario
Usuario → TGS: TGT + solicitud de acceso al servicio S
TGS → Usuario: Ticket de servicio para S
Usuario → S: Ticket de servicio
```

## Ventajas

- Las contraseñas nunca viajan por la red
- Soporte nativo de [[SSO - Single Sign-On]]
- Auditoría centralizada

## Limitaciones

- Requiere sincronización de relojes (tolerancia típica de 5 minutos)
- El KDC es un punto único de fallo
- No está diseñado para entornos abiertos como internet (para eso existe [[TLS]] + [[PKI]])

## Ver también

- [[Protocolo Needham-Schroeder]]
- [[Key Distribution Centers]]
- [[SSO - Single Sign-On]]
- [[Autenticación]]
