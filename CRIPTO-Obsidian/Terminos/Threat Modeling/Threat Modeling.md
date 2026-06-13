El **modelado de amenazas** es una técnica de ingeniería que se aplica para entender las amenazas, ataques, vulnerabilidades y contramedidas que pueden afectar a una aplicación.

## Objetivos

- Mejorar el **diseño** del sistema.
- Reducir el **riesgo** ante un ataque.
- Definir **contramedidas específicas** para cada tipo de amenaza identificada.

## Características

- Debe incluir información de **inteligencia, tendencias y ataques pasados**.
- Es un **proceso continuo**: las amenazas cambian constantemente.
- Define la tarea de modelar las amenazas en la **superficie de ataque**.

## Proceso (OWASP)

1. **Definir el alcance**: acotar el esfuerzo para que cubra lo suficiente sin excederse.
2. **Determinar las amenazas**.
3. **Definir contramedidas y mitigación**.
4. **Revisar** el trabajo realizado, los cambios, y volver a empezar.

https://owasp.org/www-community/Threat_Modeling_Process

## Herramientas y frameworks

| Herramienta | Descripción |
|---|---|
| [[Modelo STRIDE]] | Framework de Microsoft para identificar tipos de amenazas |
| [[Data Flow Model (DFD)]] | Modelo para representar las partes del sistema y sus riesgos |

## Gestión del riesgo identificado

Por cada riesgo identificado se debe definir cómo gestionarlo:

| Estrategia | Descripción |
|---|---|
| **Eliminar** | Rediseñar para que la amenaza no aplique |
| **Mitigar** | Reducir la probabilidad o el impacto |
| **Compensar** | Agregar controles alternativos |
| **Aceptar** | Documentar y asumir el riesgo conscientemente |

## Manifesto

https://www.threatmodelingmanifesto.org/

## Referencia Microsoft

https://www.microsoft.com/en-us/securityengineering/sdl/threatmodeling
