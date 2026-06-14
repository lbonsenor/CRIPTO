![[2-2025-Q1-2-EJ3.png]]
**Conceptos:**
- [[Salting]]
- [[Fórmula de Anderson]]
- [[Ingeniería Social en PenTest]]
- [[Factor - Algo que conozco]]
- [[Factor - Algo que soy]]

### a)
$P\approx  \frac{T\cdot G}{N}$
$P=0.5$
$\frac{31536000\text{ seg}\cdot 10^4\text{ p/s}}{62^5}=344.229\implies P=1$

**José esta equivocado**, esto es por que el tiempo otorgado (T) es tan absurdamente grande en comparación con el tamaño del espacio de claves (N) que el atacante tiene capacidad matemática de agotar y recorrer por completo todo el espacio de combinaciones **múltiples veces** dentro de ese año.

### b)
El canal de SMS depende enteramente de la infraestructura de las empresas de telefonía celular, la cual no es inherentemente segura. El principal riesgo es el **SIM Swapping** (suplantación de identidad telefónica): un atacante utiliza ingeniería social sobre la operadora telefónica para clonar la tarjeta SIM de la víctima en un nuevo chip.

No cumple con el principio de **Independencia de Factores** (o aislamiento de canales).
La autenticación multifactor efectiva se basa en combinar cosas de categorías distintas:
- **Algo que sabés:** Contraseña.
- **Algo que tenés:** Teléfono físico / Token.

Al permitir que el elemento de recuperación ("algo que tenés" / SMS) reemplace o reinicie el elemento de conocimiento ("algo que sabés" / Contraseña), **los dos canales colapsan en uno solo**. Si el atacante compromete el canal SMS, automáticamente gana acceso al control total de la cuenta, destruyendo el propósito de tener una arquitectura de autenticación robusta y distribuida.