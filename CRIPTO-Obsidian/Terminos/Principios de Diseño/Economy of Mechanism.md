> Principio 3 de [[Principios de Diseño de Seguridad]]

**Simplicidad** establece que se debe evitar el uso de arquitecturas muy sofisticadas al desarrollar controles de seguridad.

## Fundamento

- Los sistemas muy complejos **aumentan el riesgo de errores** y la dependencia de otros componentes para aplicar el control.
- Cada componente adicional es un potencial punto de falla o ataque.

## Reducción de la superficie de ataque

El principio incluye también la **complejidad de datos**: reducir al mínimo la cantidad de tuplas `(sujeto, acceso, objeto)`.

> Cada uno de esos accesos es susceptible a ser explotado. Al reducir la cantidad de accesos disponibles para un sujeto, se reduce el riesgo de un ataque efectivo.

## Implicaciones prácticas

- Preferir soluciones simples y bien entendidas sobre soluciones sofisticadas y difíciles de auditar.
- Revisar y eliminar regularmente permisos y accesos que ya no son necesarios.
- Evitar dependencias innecesarias entre componentes de seguridad.

## Relación con otros conceptos

- [[Least Privilege]]: reducir los accesos de cada sujeto es una forma de aplicar este principio.
- [[Separation of Privilege]]: la segregación debe mantenerse simple para ser efectiva.
- [[Threat Modeling]]: ayuda a identificar la complejidad innecesaria.
