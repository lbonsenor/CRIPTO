Busca solucionar los problemas de [[RSA-Signature]], donde:

$\text{Sign}_{sk}(m)\equiv H(m)^d\pmod{n}$
$\text{Vrfy}_{pk}(m,s)\equiv H(m)=s^e\pmod{n}$

No es seguro a menos que se asuma un modelo ideal de $H$