### Ventajas

- **Integridad del texto plano oculta:** El MAC se calcula directamente sobre el mensaje original y viaja cifrado. Un atacante no puede ver el MAC resultante, lo que en teoría impide que intente analizar patrones directamente en la etiqueta de autenticación.
### Desventajas
- **Vulnerable a Oráculos de Relleno:** Esta es su mayor debilidad (y la razón de la caída de versiones antiguas de TLS/SSL). Para verificar el MAC, el receptor _primero debe descifrar_ el mensaje. Si el descifrado falla debido a un formato de relleno incorrecto y el sistema devuelve un error diferente al de "MAC fallido", el atacante puede deducir información del texto plano.
    
- **Desperdicio de recursos (DoS):** El receptor debe realizar todo el proceso criptográfico de descifrado (que consume mucha CPU) antes de poder verificar si el mensaje era auténtico o si fue modificado por un atacante.