**Open Policy Agent (OPA)** es un framework estándar que unifica, por medio de un lenguaje programable, la implementación de políticas de decisión.

## Características principales

- **Desacopla** la implementación de políticas de [[Control de Acceso]] del sistema que las aplica (_policy decoupling_).
- Permite definir políticas centralizadas que pueden ser consultadas por múltiples servicios.
- Es especialmente útil en entornos donde las [[Reglas basadas en Contexto]] son complejas de implementar dentro del sistema mismo.
- Es parte del modelo [[Zero-Trust]], donde una entidad externa (el Policy Engine) evalúa todas las condiciones de acceso.

## Arquitectura

```
Servicio  ──── Query (JSON) ──►  OPA  ◄──── Política (Rego)
                                  │
                           Data (contexto)
                                  │
                           Decisión (allow/deny)
```

- El servicio envía un **query** con información del requerimiento (cualquier JSON).
- OPA evalúa el query contra la **política** definida en [[Rego]] y los **datos de contexto**.
- OPA devuelve una **decisión** sobre el acceso.
- Los datos de contexto se actualizan regularmente o se proveen _on-demand_.

## Casos de uso

- Autorización en microservicios y APIs.
- Políticas en Kubernetes (via Admission Controllers).
- Control de acceso en infraestructura (Terraform, etc.).

## Referencia
https://www.openpolicyagent.org/