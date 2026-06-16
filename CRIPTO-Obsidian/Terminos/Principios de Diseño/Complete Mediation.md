> Principio 4 de [[Principios de Diseño de Seguridad]]

**Mediación completa** establece que si existe un control de seguridad, este debe aplicarse a **todas** las interacciones que alcance, sin excepción.

## Concepto

La existencia de cualquiera de los siguientes elementos representa un problema de seguridad (**partial mediation**):

- Muestreo de algunas transacciones (no todas).
- Controles al azar.
- Entidades o flujos que quedan por fuera del control.

## Consecuencias de la mediación parcial

Un atacante puede identificar los canales o momentos donde el control no aplica y explotarlos sistemáticamente.

## Ejemplos

- Un firewall que filtra el tráfico HTTP pero no HTTPS.
- Un sistema de [[Autenticación]] que verifica identidad en el login pero no revalida en operaciones críticas posteriores.
- Un [[API Gateway]] que controla las rutas públicas pero deja una ruta interna sin proteger.

## Relación con otros conceptos

- [[Least Privilege]]: de nada sirve asignar mínimos privilegios si el control no aplica en todos los accesos.
- [[Mandatory Access Control]]: implementación de mediación completa a nivel de sistema operativo.
- [[Open Policy Agent]]: herramienta para centralizar y garantizar mediación completa en múltiples servicios.
- [[Zero-Trust]]: modelo que formaliza la verificación en cada acceso, sin excepciones.
