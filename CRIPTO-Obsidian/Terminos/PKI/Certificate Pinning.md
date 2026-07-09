Técnica usada principalmente en aplicaciones móviles donde la app tiene "grabado" (pinned) el certificado o la clave pública esperada de su servidor, en vez de confiar en cualquier certificado válido según la [[Cadena de Confianza]] del sistema operativo.

Previene un [[Ataque Man-In-The-Middle]] incluso si el atacante logra instalar una CA maliciosa en el dispositivo (por ejemplo mediante un proxy de intercepción como Burp Suite): la app rechaza la conexión si el certificado presentado no coincide exactamente con el pinneado, sin importar que esa CA sea confiable para el sistema operativo.
