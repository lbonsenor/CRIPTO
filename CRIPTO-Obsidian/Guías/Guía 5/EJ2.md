> [!note] Enunciado
> Considera el siguiente protocolo para enviar un texto plano $M$ entre A y B:
> 1. $A \to B: \{ K_{pA} \}$
> 2. $B \to A: \{ K_{pB} \}$
> 3. $A \to B: \{ E_B(M) \}$
> 4. $B \to A: \{ E_A(M) \}$
>
> Si un adversario Z intercepta el primer mensaje, ¿cómo hace para obtener el texto plano M?

Z realiza un ataque **Man in the Middle** de la siguiente manera:

1. Captura la clave pública de A y la guarda: $A \to Z: \{ K_{pA} \}$
2. Le pasa a B su propia clave pública: $Z \to B: \{ K_{pZ} \}$
3. B le devuelve su clave pública, creyendo que se la manda a A: $B \to Z: \{ K_{pB} \}$
4. Z se queda con $K_{pB}$ y le envía a A su propia clave pública: $Z \to A: \{ K_{pZ} \}$

Así, Z tiene el control total de la comunicación, porque tanto A como B encriptarán sus mensajes con $K_{pZ}$, y solo Z puede desencriptarlos con su clave privada. Luego puede reenviarlos al otro con modificaciones:

$$\begin{align}
A \to B(Z)&: \{ E_Z(M) \} \\
A(Z) \to B&: \{ E_B(M') \} \\
B \to A(Z)&: \{ E_Z(R) \} \\
B(Z) \to A&: \{ E_A(R') \}
\end{align}$$
