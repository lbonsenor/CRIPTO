> Principio 6 de [[Principios de Diseño de Seguridad]]

**Segregación de tareas** establece que toda tarea crítica **no debe quedar bajo el control de un solo sujeto**. Más de un sujeto debe ser parte del proceso.

## Control por oposición de intereses

Una forma de aplicar este principio es mediante el **control por oposición de intereses**: las partes que participan tienen objetivos opuestos, y en ese equilibrio se evita el abuso.

### Ejemplo: proceso de ventas con descuentos

- El **vendedor** necesita la aprobación de un descuento. Le conviene descontar mucho para asegurar la venta.
- El **financiero** que aprueba el descuento quiere descontar lo menos posible para no perder margen.
- La oposición de intereses entre ambas partes actúa como control natural.

## Ejemplos en sistemas

- Un commit de código requiere revisión de otro desarrollador (_pull request_).
- Una transferencia bancaria de alto valor requiere aprobación de un segundo operador.
- La emisión de un certificado digital requiere múltiples firmas.

## Relación con otros conceptos

- [[Least Privilege]]: cada parte involucrada solo tiene los permisos necesarios para su rol.
- [[Defense in Depth]]: la segregación es una capa más en la defensa en profundidad.
- [[Mandatory Access Control]]: puede implementar segregación obligatoria.
- [[Modelo Clark-Wilson]]: modelo de seguridad que formaliza la separación de tareas.
