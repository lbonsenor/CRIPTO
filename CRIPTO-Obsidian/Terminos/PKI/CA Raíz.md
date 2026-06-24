[[Autoridad Certificante]] que **firma su propio certificado** (*self-signed certificate*). Es el punto de confianza máximo en una [[Cadena de Confianza]].

## Características

- No tiene emisor externo: su certificado es auto-firmado
- Su clave pública debe ser conocida de antemano por los clientes
- Es el ancla de confianza (*trust anchor*) del sistema
- Suele operar **offline** por razones de seguridad

## Distribución

Los certificados raíz vienen **preinstalados** en:
- Sistemas operativos (Windows, Linux, macOS)
- Navegadores web (Chrome, Firefox, Safari)
- Runtimes como la JVM

Ejemplos de CAs raíz conocidas: DigiCert, Let's Encrypt (ISRG Root), Comodo, GlobalSign.

## Por qué confiar en ellas

La confianza en las CAs raíz es una **decisión de política**: los fabricantes de sistemas operativos y navegadores deciden qué CAs incluir. Si una CA raíz es comprometida, todos los certificados emitidos bajo ella son potencialmente inválidos.

## Relación con la jerarquía

```
CA Raíz (auto-firmada)
  └── CA Intermedia (firmada por la raíz)
        └── Certificado de servidor (firmado por la intermedia)
```

Las CAs intermedias actúan como buffer: si se compromete una intermedia, la raíz (offline) puede revocarla sin afectar al resto de la jerarquía.

## Ver también

- [[Autoridad Certificante]]
- [[Cadena de Confianza]]
- [[PKI]]
- [[Certificado X.509]]
