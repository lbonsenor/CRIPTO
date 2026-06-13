**OWASP** (_Open Web Application Security Project_) es una organización focalizada en combatir los problemas de seguridad en aplicaciones web.

## Función

Publica regularmente las **vulnerabilidades web más comunes** y las buenas prácticas para prevenirlas, con foco en la comunidad de desarrolladores.

## Listas publicadas

| Lista | Alcance | Referencia |
|---|---|---|
| **OWASP Top 10** | Aplicaciones web en general | https://owasp.org/Top10/ |
| **OWASP Top 10 LLM** | Vulnerabilidades específicas de modelos de lenguaje (LLMs) | https://genai.owasp.org/llm-top-10/ |
| **OWASP Cloud Top 10** | Vulnerabilidades en entornos cloud | [[Cloud Vulnerabilities]] |

## OWASP Top 10 LLM

Foco en las vulnerabilidades más comunes en sistemas basados en LLMs (como ChatGPT). Incluye categorías como prompt injection, insecure output handling, training data poisoning, entre otras.

## Proceso de Threat Modeling con OWASP

OWASP propone un proceso de [[Threat Modeling]] con cuatro pasos:

1. **Definir el alcance**: acotar el esfuerzo para que cubra lo suficiente sin excederse.
2. **Determinar las amenazas**.
3. **Definir contramedidas y mitigación**.
4. **Revisar** el trabajo, los cambios, y volver a empezar (proceso continuo).

## Relación con otros conceptos

- [[CWE]]: las categorías del Top 10 mapean sobre CWEs específicas.
- [[Amenazas y Vulnerabilidades]]: marco general.
- [[Threat Modeling]]: proceso donde se aplica el Top 10 como referencia.
- [[Cloud Vulnerabilities]]: lista específica para entornos cloud.
