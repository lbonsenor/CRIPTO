> [!note] Enunciado
> Considera el siguiente protocolo de autenticación mutua:
> 1. $A \to B: N_1$
> 2. $B \to A: N_2$
> 3. $A \to B: (N_2)_{K_s}$
> 4. $B \to A: (N_1)_{K_s}$
>
> donde $N_1$, $N_2$ son nonces y $K_s$ es una clave simétrica compartida entre A y B.
>
> ¿Qué situación NO debe permitir A para evitar que un tercero no autorizado se autentique correctamente?

A **no debe permitir que Z reenvíe el mismo nonce** en una sesión nueva. 

Si A acepta el mismo $N_1$ en distintas sesiones, un atacante Z que haya capturado previamente el mensaje $(N_1)_{K_s}$ podría reenviarlo como si fuera la respuesta legítima del paso 4, autenticándose correctamente sin conocer $K_s$.

**Solución:** A debe generar un nonce $N_1$ fresco y distinto en cada sesión, y rechazar respuestas que no correspondan al nonce generado en esa sesión específica.
