La **Zero Trust Architecture (ZTA)** es el modelo de seguridad empresarial que asume que las amenazas, tanto internas como externas, están **presentes en la red en todo momento**.

> Extiende el concepto general de [[Zero-Trust]] con una arquitectura concreta de componentes.

## Principios básicos

| Principio | Descripción |
|---|---|
| **Never Trust** | No se otorga confianza implícita por ubicación o red |
| **Always Verify** | Cada acceso se verifica explícitamente, siempre |
| **Assume Breach** | Se opera asumiendo que el sistema ya fue comprometido |

## Objetivo

Proteger la integridad, confidencialidad y disponibilidad de los datos y recursos de la organización independientemente de dónde se encuentren los usuarios o los activos ([[La triada]]).

## Componentes

Ver: [[ZTA - Componentes]]

Los componentes principales incluyen: CDM, Threat Intelligence, Logs y SIEM, IAM, PKI, políticas de acceso.

## Limitaciones de Defense-in-Depth que ZTA resuelve

La estrategia [[Defense in Depth]] tradicional presenta problemas en entornos modernos:
- No resuelve eficientemente ataques internos.
- Difícil de implementar cuando los sistemas están distribuidos (datacenters híbridos, múltiples nubes, empresas cloud-native).

ZTA aborda estos problemas eliminando el concepto de "perímetro de red confiable".

## Implementaciones de referencia

- **NIST SP 800-207**: https://csrc.nist.gov/pubs/sp/800/207/final
- **Azure Zero Trust**: https://learn.microsoft.com/en-us/azure/security/fundamentals/zero-trust
- **AWS Zero Trust**: https://docs.aws.amazon.com/prescriptive-guidance/latest/strategy-zero-trust-architecture/
- **Red Hat / Linux**: https://www.redhat.com/en/resources/rhel-for-zero-trust-architectures-ebook

## Relación con otros conceptos

- [[Zero-Trust]]: el principio general del que ZTA es la implementación empresarial.
- [[Open Policy Agent]]: herramienta para implementar el Policy Engine de ZTA.
- [[Privileged Access Management]]: gestión de accesos privilegiados dentro de ZTA.
- [[Multi-Factor Authentication]]: requisito de verificación continua en ZTA.
- [[Risk Management]]: ZTA es una estrategia de gestión de riesgos.
