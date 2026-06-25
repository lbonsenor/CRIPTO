### Ventajas
- **Inmunidad a ataques de oráculo de relleno (Padding Oracle Attacks):** Como el MAC se valida _antes_ de intentar descifrar el mensaje, cualquier modificación en el texto cifrado hará que el MAC falle. El receptor descarta el mensaje inmediatamente sin llegar a la fase de descifrado, evitando fugas de información sobre el relleno.
    
- **Eficiencia en el descarte de ataques:** Si un atacante inyecta mensajes maliciosos o corruptos en la red, el receptor los detecta y descarta rápidamente mediante una verificación de MAC muy ligera, protegiendo al sistema de ataques de Denegación de Servicio (DoS) que buscan agotar la CPU descifrando basura.
    
- **Seguridad demostrable:** Es el único método que garantiza teóricamente la "seguridad contra ataques de texto cifrado escogido" (IND-CCA2) bajo cualquier combinación de algoritmos de cifrado y MAC seguros.

### Desventajas

- **Gestión de claves estrictas:** Requiere obligatoriamente el uso de dos claves independientes (una para el cifrado y otra para el MAC). Si se usa la misma clave para ambos procesos, se pueden introducir vulnerabilidades graves dependiendo de los algoritmos utilizados.