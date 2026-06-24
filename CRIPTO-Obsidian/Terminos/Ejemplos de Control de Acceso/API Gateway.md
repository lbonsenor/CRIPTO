Un **API Gateway** es un servicio que se despliega **delante de las APIs** para ofrecer una capa adicional de seguridad y funcionalidad.

## Funciones principales

- Define controles de acceso basados en reglas ([[Mandatory Access Control]]).
- Actúa como punto único de entrada para múltiples APIs backend.

## Parámetros de control de acceso

Las reglas del API Gateway pueden involucrar:

| Parámetro | Descripción |
|---|---|
| **URL** | Ruta del recurso solicitado |
| **Dominio** | Origen de la solicitud |
| **Headers** | Cabeceras HTTP (ej: `Authorization`) |
| **Method** | Método HTTP (GET, POST, PUT, DELETE...) |

## Control de acceso por contexto

Adicionalmente puede definir reglas basadas en [[Reglas basadas en Contexto]]:

- **Rate limiting**: cantidad máxima de requerimientos por segundo desde un IP o SessionID.
- **Bandwidth limiting**: ancho de banda máximo consumido.

Si se superan estos umbrales, se bloquean nuevas conexiones del mismo origen.

## Relación con OAuth

Los API Gateways frecuentemente validan tokens de [[OAuth 2.0]] como parte del control de acceso, verificando que el token sea válido y contenga los _scopes_ requeridos.

## Relación con otros conceptos
- [[Mandatory Access Control]]: las reglas se aplican a todos los accesos.
- [[RuBAC]]: el sistema de reglas es una implementación de control basado en reglas.
- [[Reglas basadas en Contexto]]: rate limiting y bandwidth limiting.
- [[OAuth 2.0]]: mecanismo de autorización complementario.
