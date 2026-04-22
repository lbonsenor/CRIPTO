![[EJ5.png]]
### a) **Falso**

- **Corrección:** MD5 es una **función hash (no es un criptosistema de encriptación)** y no debe ser utilizado porque es **criptográficamente inseguro debido a vulnerabilidades de colisión**, no por su longitud de 128 bits.
    
- **Cambio realizado:** Se cambió "criptosistema de encriptación asimétrico" por "función hash" y la justificación de la inseguridad (de longitud de clave a vulnerabilidad de colisiones).
    

---

### b) **Falso**

- **Corrección:** Un protocolo de autenticación basado únicamente en un MAC simétrico provee **autenticidad e integridad**, pero **no provee confidencialidad ni no repudio**.
    
- **Cambio realizado:** Se eliminó "confidencialidad" y "no repudio" de las propiedades provistas. El no repudio no se puede lograr con algoritmos simétricos (como MAC) porque ambas partes conocen la misma clave.
    

---

### c) **Verdadero**

- **Explicación:** El uso de esquemas de _padding_ probabilísticos (como OAEP - _Optimal Asymmetric Encryption Padding_) es fundamental en RSA para lograr la seguridad semántica frente a ataques de texto cifrado elegido (IND-CCA2). Sin un _padding_ adecuado, el RSA de "libro" es determinista y vulnerable.
    

---

### d) **Falso**

- **Corrección:** Un certificado digital emitido por una autoridad certificante (CA) contiene siempre la **clave pública del sujeto (titular) al que pertenece el certificado**, no la clave pública de la propia CA.
    
- **Cambio realizado:** Se cambió "la clave pública de la CA" por "la clave pública del sujeto/titular". La CA sí incluye su **firma digital** (generada con su clave privada), pero el certificado en sí es el documento que vincula a un sujeto con su clave pública.