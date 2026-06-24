Entidad de confianza responsable de emitir, firmar y administrar [[Certificado Digital|certificados digitales]]. También llamada **CA** (*Certificate Authority*) o **AC**.

## Rol

- Verifica la identidad del solicitante antes de emitir un certificado
- Firma el certificado con su propia [[Criptosistema Asimétrico|clave privada]]
- Mantiene la [[CRL - Certificate Revocation List|lista de revocación]] de certificados emitidos
- Puede delegar capacidad de firma a CAs subordinadas

## Jerarquía de CAs

Las CAs se organizan en una jerarquía:

```
CA Raíz (Root CA)
  └── CA Intermedia
        └── CA Emisora
              └── Certificado final
```

Cada nivel firma los certificados del nivel inferior, formando una [[Cadena de Confianza]].

## CA Raíz

Es la [[CA Raíz|autoridad que firma su propio certificado]] (*self-signed*). Es el **punto de confianza** del sistema. Su certificado viene preinstalado en:
- Sistemas operativos
- Navegadores web
- Runtimes como la JVM

## Validación entre CAs distintas

Si A y B usan CAs diferentes, existen dos opciones:
1. A confía explícitamente en la CA de B
2. Las CAs se **certifican mutuamente** (cada una emite un certificado de la otra)

En la práctica, los sistemas operativos y navegadores mantienen listas de CAs reconocidas globalmente.

## Tipos de certificados emitidos

- **DV** (Domain Validation): solo verifica control del dominio
- **OV** (Organization Validation): verifica también la organización
- **EV** (Extended Validation): verificación extendida, alta confianza

## Ver también

- [[CA Raíz]]
- [[Cadena de Confianza]]
- [[Certificado Digital]]
- [[Certificado X.509]]
- [[CRL - Certificate Revocation List]]
- [[OCSP - Online Certificate Status Protocol]]
- [[PKI]]
