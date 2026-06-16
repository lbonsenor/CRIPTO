**BAS** (_Breach and Attack Simulation_) es una categoría de herramientas y técnicas que **simulan de forma automatizada y recurrente** las tácticas, técnicas y procedimientos (TTPs) que un atacante real emplearía para comprometer un sistema.

## Diferencia con PenTest

| Aspecto | [[PenTest]] | BAS |
|---|---|---|
| Ejecución | Manual por expertos | Automatizada |
| Frecuencia | Puntual (por proyecto o período) | **Continua y recurrente** |
| Objetivo | Explotar vulnerabilidades | Evaluar capacidad de **detección y respuesta** |
| Alcance | Definido por el evaluador | Sistemático en todos los puntos de la organización |

## Qué evalúa BAS

- La capacidad de la organización para **identificar y reaccionar** ante ataques.
- La efectividad de todas las herramientas de protección activas (firewalls, IDS/IPS, SIEM, EDR).
- Los sistemas de **monitoreo y alertas**.
- **Cambios de configuración** o procesos que hayan dejado de actuar como se esperaba (_configuration drift_).

## Características clave

- Se ejecuta en **forma recurrente**, probando distintos puntos de la organización.
- Evalúa simultáneamente: capacidad de **detección**, **mitigación** y **respuesta**.
- Complementa al [[Red Blue Purple Team]] al automatizar ataques de baja complejidad de forma continua.

## Relación con otros conceptos

- [[PenTest]]: el complemento manual y más profundo de BAS.
- [[Red Blue Purple Team]]: BAS automatiza parte del trabajo del Red Team.
- [[Risk Management]]: los resultados de BAS alimentan la evaluación y monitoreo de riesgos.
- [[Defense in Depth]]: BAS verifica que cada capa de la defensa en profundidad funciona correctamente.
- [[Malware Attack Cycle]]: BAS simula las TTPs de cada etapa del ciclo de ataque.
