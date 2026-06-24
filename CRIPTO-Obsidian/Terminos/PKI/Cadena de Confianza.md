Secuencia de [[Certificado Digital|certificados]] enlazados por [[Firma Digital|firmas digitales]], desde un certificado final hasta una [[CA Raíz]] de confianza. Es el mecanismo central de verificación en [[PKI]].

## Estructura

Cada certificado en la cadena está firmado por el certificado anterior:

```
[Cert. del usuario/servidor]
        ↑ firmado por
[Cert. CA Intermedia]
        ↑ firmado por
[Cert. CA Raíz]  ← preinstalado, punto de confianza
```

## Notación

Se acostumbra incluir el certificado de la [[Autoridad Certificante]] junto con el certificado del sujeto. Ejemplo:

- Certificado de A, firmado por CA3: `Ca = C'a || C_CA3 || C_CA2 || C_CA1`
- Certificado de B, firmado por CA1: `Cb = C'b || C_CA1`

## Proceso de validación

1. Verificar la [[Firma Digital]] del certificado del sujeto usando la clave pública de su CA emisora
2. Si esa CA no es raíz, verificar su certificado recursivamente
3. Continuar hasta llegar a una [[CA Raíz]] conocida y de confianza

## Por qué existe

La [[CA Raíz]] no firma directamente los certificados de usuarios finales por razones de seguridad (opera offline). Delega en CAs intermedias. Esto también permite:
- Revocar una CA intermedia comprometida sin afectar a la raíz
- Segmentar la emisión por geografía, tipo de certificado, etc.

## Ver también

- [[Autoridad Certificante]]
- [[CA Raíz]]
- [[Certificado Digital]]
- [[Certificado X.509]]
- [[CRL - Certificate Revocation List]]
