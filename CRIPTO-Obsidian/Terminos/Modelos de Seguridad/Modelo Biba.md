- Modelo diseñado para proteger la **integridad de la información**. 
- Define que la **información debe clasificarse** de acuerdo al nivel de integridad de la misma. 
- Todos los sujetos deben tener un **nivel de integridad** definido, con los mismos posibles niveles de la información. 

> [!info] Propiedad de Integridad Simple (Simple Integrity Property)
> **"No leer hacia abajo" (No Read Down)**
>
>Un sujeto **no puede leer** información de un nivel inferior al suyo.
>
>- **El motivo:** Si un proceso del sistema altamente confiable (nivel alto) lee datos corruptos o sin verificar de internet (nivel bajo), ese proceso podría infectarse o tomar decisiones basadas en información falsa.
    
> [!info] Propiedad de la Estrella de Integridad ($*$-Integrity Property)
>
> **"No escribir hacia arriba" (No Write Up)**
>
>Un sujeto **no puede escribir** ni modificar objetos en un nivel superior al suyo.
>
>- **El motivo:** Un usuario común o un archivo descargado de internet (nivel bajo) no debe tener permisos para modificar los archivos de configuración del sistema operativo (nivel alto), ya que rompería la integridad de la máquina.

### Diferencias con [[Modelo Bell-La Padula]]

| Caracteristica         | Bell-La Padula                                         | Biba                                                   |
| ---------------------- | ------------------------------------------------------ | ------------------------------------------------------ |
| **Objetivo Principal** | **Confidencialidad** (Evitar filtraciones).            | **Integridad** (Evitar modificaciones no autorizadas). |
| **Regla de Lectura**   | **No Read Up** (No lees niveles superiores).           | **No Read Down** (No lees niveles inferiores).         |
| **Regla de Escritura** | **No Write Down** (No escribes en niveles inferiores). | **No Write Up** (No escribes en niveles superiores).   |
| **Filosofía**          | Mantener el secreto a toda costa.                      | Mantener los datos limpios y confiables.               |
| **Contexto ideal**     | Sistemas militares y gubernamentales.                  | Sistemas bancarios, médicos o industriales.            |
