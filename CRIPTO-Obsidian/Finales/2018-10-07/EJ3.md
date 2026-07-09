> [!example] Enunciado
> Alegría S.A. (fábrica de juguetes) integra su catálogo e inventario con la plataforma de ecommerce de Comercialus S.A. Para el inventario, expone a internet una URL llamada por Comercialus ante cada venta o devolución. El servicio es una app autocontenida con su propio web server, corriendo en una computadora de Alegría (empresa sin equipo de IT, sin servidores propios).
>
> Establecer 4 hipótesis de vulnerabilidades con alta probabilidad de existir, con hipótesis y prueba para cada una.

**Conceptos:**

* [[CWE-89 - SQL Injection]]
* [[Improper Authentication]]

**Hipótesis 1: Falta de autenticación en el webhook — Falsificación de ventas/devoluciones**
- Hipótesis: La URL expuesta a internet es pública; si el servicio no valida que el request viene de Comercialus (token secreto, firma, whitelist de IPs), un atacante puede fabricar ventas o devoluciones falsas.
- Prueba: Enviar desde afuera un `POST /webhook/venta` simulando una venta y verificar si se acepta sin validar el origen.

**Hipótesis 2: Computadora sin hardening expuesta a internet**
- Hipótesis: Sin equipo de IT, es probable que la computadora tenga puertos innecesarios abiertos, servicios por defecto, sistema sin actualizar y sin firewall.
- Prueba: Escanear puertos con `nmap -sV -p 1-65535 [IP]` contra la IP pública y verificar qué servicios además del web server están expuestos.

**Hipótesis 3: [[CWE-89 - SQL Injection|SQL Injection]] en los parámetros del webhook**
- Hipótesis: Si los parámetros de venta/devolución no están sanitizados ni parametrizados al actualizar el inventario, se puede inyectar SQL.
- Prueba: Enviar al webhook un parámetro tipo `"producto": "' OR '1'='1"` y verificar si se procesa la inyección.

**Hipótesis 4: API keys de Comercialus hardcodeadas o expuestas**
- Hipótesis: Sin equipo de IT, es probable que las credenciales de la API de Comercialus estén hardcodeadas en el código o en configuración en texto plano.
- Prueba: Si se logra acceso a la computadora, buscar (`grep -r "api_key\|token\|password"`) archivos de configuración o código fuente con credenciales expuestas.
