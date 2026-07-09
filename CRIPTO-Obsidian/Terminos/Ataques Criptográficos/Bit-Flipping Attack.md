Ataque que explota la falta de integridad de un cifrado. En modos como [[CBC]] o cifrados de flujo (XOR con keystream), modificar bits del texto cifrado produce cambios predecibles y controlados en el texto plano correspondiente al descifrar, sin que el receptor lo note — porque el cifrado por sí solo no garantiza que el mensaje no fue alterado.

Se previene agregando un [[Message Authentication Code]] con clave independiente de la del cifrado, o usando cifrado autenticado como [[GCM - Galois Counter Mode]], que detecta cualquier modificación del texto cifrado.
