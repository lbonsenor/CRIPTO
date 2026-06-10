**Zero-Trust** es un modelo de seguridad basado en el principio de **"nunca confiar, siempre verificar"**: ningún sujeto, dispositivo o red es considerado confiable por defecto, incluso si está dentro del perímetro de la organización.

## Principio fundamental

> No se otorga acceso implícito por ubicación (red interna, VPN, etc.). Cada acceso debe ser verificado explícitamente.

## Policy Engine externo

En Zero-Trust se define una entidad externa al sistema denominada **Policy Engine**, responsable de evaluar **todas las condiciones de contexto** antes de otorgar acceso.

- Resuelve la dificultad de implementar [[Reglas basadas en Contexto]] en sistemas que no pueden modificarse.
- Herramientas como [[Open Policy Agent]] implementan este rol.

## Relación con otros modelos

| Concepto | Rol en Zero-Trust |
|---|---|
| [[Mandatory Access Control]] | Base del control obligatorio |
| [[Reglas basadas en Contexto]] | Condiciones dinámicas evaluadas |
| [[Open Policy Agent]] | Implementación del Policy Engine |
| [[OAuth 2.0]] | Mecanismo de autorización compatible |
