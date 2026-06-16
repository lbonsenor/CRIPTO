**Rego** es el lenguaje de políticas utilizado por [[Open Policy Agent]].

## Características
- Proporciona una forma **flexible y potente** de definir políticas para diversos casos de uso.
- Combina reglas y operadores/funciones integradas para crear políticas comprensivas.
- Permite imponer restricciones y permisos dentro de aplicaciones e infraestructura.
- Es un lenguaje **declarativo** orientado a consultas.

## Ejemplo básico
```rego
default allow := false

allow if {
    creditor_is_valid
    debtor_is_valid
    period_is_valid
    amount_is_valid
}

creditor_is_valid if
    data.account_attributes[input.creditor_account].owner == input.creditor

debtor_is_valid if
    data.account_attributes[input.debtor_account].owner == input.debtor

period_is_valid if
    input.period <= 30

amount_is_valid if
    data.account_attributes[input.debtor_account].amount >= input.amount
```

- `input`: datos del requerimiento enviados por el servicio (query JSON).
- `data`: datos de contexto externos (se actualizan regularmente).
- `default allow := false`: por defecto, deniega el acceso.

## Ejemplo en Kubernetes

Rego se usa frecuentemente con **OPA Gatekeeper** en Kubernetes para validar recursos antes de que sean creados en el cluster.

## Referencia
https://www.openpolicyagent.org/docs/latest/policy-language/