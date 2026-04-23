![[Parciales/Primer Parcial/2025-Q1/Images/EJ5.png]]

a) Verdadero

b) Los sistemas de clave pública utilizan un par de claves matemáticamente relacionadas: una para realizar una operación y la otra para su inversa. Para **cifrar**, se usa la clave pública del receptor y este **desencripta** con su clave privada. Para **firmar**, el emisor usa su clave privada y el receptor **verifica** con la clave pública del emisor.

c) El número de cifrados de **sustitución** es inmensamente mayor. Para un código de 3 bits ($2^3=8$ estados posibles), un cifrado de sustitución permite $8!=40320$ combinaciones. En cambio, un cifrado de transposición solo permite permutar la posición de los 3 bits, lo que da solo $3!=6$ combinaciones posibles.

d) Un certificado público contiene la **clave pública** del dueño del certificado (la entidad certificada). Este documento está **firmado digitalmente** por la entidad certificante (CA) usando la clave privada de dicha CA, pero nunca contiene la clave privada de nadie.