> Principio 5 de [[Principios de Diseño de Seguridad]]

**Sistema abierto** establece que la seguridad de un sistema **no debe basarse en mantener su diseño oculto**.

## Fundamento

> Todo secreto conocido por al menos una persona puede ser revelado.

Basar la seguridad en que el mecanismo sea desconocido (_security through obscurity_) es frágil: si el diseño se filtra, la seguridad se pierde por completo.

## Aplicación en criptografía

Este principio es la base de los [[Principios de Kerchoffs]]:

- Los algoritmos de cifrado son **públicos y conocidos por todos**.
- Lo que se guarda en secreto es únicamente la **clave**, que puede cambiarse frecuentemente.
- Algoritmos como [[Data Encryption Standard (DES)]], [[AES]] y [[Textbook RSA]] son completamente públicos.

## Aplicación en controles de seguridad

El mismo concepto aplica a cualquier control de seguridad: su fortaleza debe residir en la **clave, el secreto o el token**, no en que nadie sepa cómo funciona el mecanismo.

## Relación con otros conceptos

- [[Principios de Kerchoffs]]: formalización de este principio en criptografía.
- [[Seguridad Computacional]]: la seguridad se mide asumiendo que el algoritmo es público.
- [[Autenticación]]: las credenciales son el secreto, no el mecanismo.
