Los **principios de diseño de seguridad** son bases que promueven un diseño con resultado robusto y seguro.

## Características generales

- Son principios **guía de alto nivel** que deben adaptarse a las necesidades puntuales.
- Se basan en las recomendaciones de los [[Modelo de Seguridad|modelos de seguridad]].
- Deben aplicarse en **todas las capas** del sistema.
- Son **simples y restrictivos**.
- No deben perder la perspectiva de cómo se implementan ni de quiénes los van a implementar y usar.

## Principios de Saltzer & Schroeder

Propuestos en **1975** para el diseño de sistemas operativos seguros, demostraron aplicabilidad en diseños generales y sistemas modernos. Son la base de buenas prácticas modernas y estrategias de gobierno en todas las capas.

| # | Principio | Archivo |
|---|---|---|
| 1 | Menor privilegio | [[Least Privilege]] |
| 2 | Fallar de forma segura | [[Fail-Safe Defaults]] |
| 3 | Simplicidad | [[Economy of Mechanism]] |
| 4 | Mediación completa | [[Complete Mediation]] |
| 5 | Sistema abierto | [[Open Design]] |
| 6 | Segregación de tareas | [[Separation of Privilege]] |
| 7 | Mecanismos exclusivos | [[Defense in Depth]] |
| 8 | Menor asombro | [[Psychological Acceptability]] |

## Aplicación práctica

El modelado de amenazas ([[Threat Modeling]]) permite identificar dónde aplicar estos principios en un sistema concreto.
