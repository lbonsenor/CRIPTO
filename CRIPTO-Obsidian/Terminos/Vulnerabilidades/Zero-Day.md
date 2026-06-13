Una vulnerabilidad **zero-day** es una vulnerabilidad de software descubierta pero **aún no reportada ni publicada**, para la cual no existe parche ni prevención disponible.

## Conceptos relacionados

- **Exploit**: programa que permite usar una vulnerabilidad para generar una brecha de seguridad.
- No todas las vulnerabilidades pueden ser _exploiteadas_ fácilmente.
- Una vez que una vulnerabilidad es reportada y publicada (recibe un [[MITRE CVE|CVE]]), deja de ser zero-day.

## Valor en el mercado

Los exploits zero-day tienen un **alto valor en el mercado negro y gris** porque permiten aprovechar vulnerabilidades para las cuales no hay prevención disponible. Organizaciones como **Zerodium** pagan sumas millonarias por exploits de sistemas críticos.

Ejemplos de pagos (Zerodium):
- iOS Remote Jailbreak: hasta USD 2.500.000
- Android Full Chain: hasta USD 2.500.000
- Navegadores de escritorio: hasta USD 500.000

## Ciclo de vida

```
Descubrimiento → [zero-day] → Reporte → CVE asignado → Parche → Publicación
```

Durante la etapa zero-day, los sistemas afectados no tienen forma de protegerse más allá de controles generales ([[Defense in Depth]], [[Least Privilege]]).

## Relación con otros conceptos

- [[MITRE CVE]]: el registro que se crea una vez que la vulnerabilidad se publica.
- [[Amenazas y Vulnerabilidades]]: marco general.
- [[Threat Modeling]]: ayuda a mitigar el impacto de un zero-day mediante defensa en profundidad.

## Referencia

https://zerodium.com/program.html
