Mecanismo que agrega una marca de tiempo (timestamp) y una ventana de validez a un ticket, token o mensaje, de forma que deje de ser válido después de cierto tiempo. Es la solución estándar cuando un [[Replay Attack]] es posible porque un token robado "vale para siempre".

Requiere que las partes tengan relojes razonablemente sincronizados (a diferencia de un contador, que no lo requiere). Se aplica típicamente sobre tickets tipo [[Kerberos]] o tokens de sesión.
