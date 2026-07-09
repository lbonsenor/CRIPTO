Propiedad de un [[Commitment Scheme|esquema de compromiso]] que garantiza que el receptor (B) no puede descubrir el mensaje `m` comprometido antes de la fase de reveal.

Se rompe cuando el espacio de mensajes posibles es chico: B simplemente prueba todos los valores posibles y compara contra el compromiso recibido (fuerza bruta offline). La solución estándar es concatenar un valor aleatorio (nonce) al mensaje antes de comprometerse: `h(m || r)` en vez de `h(m)`, de forma que el espacio efectivo a probar sea inmanejable.
