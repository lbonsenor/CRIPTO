Las **vulnerabilidades en la nube** presentan características particulares derivadas del modelo de responsabilidad compartida entre el proveedor y el usuario.

## Modelo de responsabilidad compartida

En servicios cloud, la responsabilidad de la seguridad se divide entre el **proveedor** (infraestructura, hipervisor, red física) y el **usuario** (configuración, aplicaciones, datos).

> Como usuario, no se puede hacer nada para prevenir, evaluar o corregir las vulnerabilidades que están del lado del proveedor.

Referencia: https://aws.amazon.com/compliance/shared-responsibility-model/

## Principales fuentes de problemas de seguridad en la nube

- **Malas prácticas** (malas configuraciones): buckets S3 públicos, grupos de seguridad permisivos, claves hardcodeadas.
- **Desconocimiento**: usuarios sin entrenamiento en seguridad cloud que toman decisiones incorrectas.
- **Problemas en lo que se despliega**: software con [[MITRE CVE|vulnerabilidades conocidas]] corriendo en la nube.
- **Uso de software con vulnerabilidades conocidas** sin aplicar parches.

## OWASP Cloud Top 10

Lista específica de las vulnerabilidades más comunes en entornos cloud-native.

https://owasp.org/www-project-cloud-native-application-security-top-10/

## Relación con otros conceptos

- [[OWASP Top 10]]: lista general de vulnerabilidades web que también aplica en la nube.
- [[Tipos de Vulnerabilidades]]: la mala configuración es la forma más común en cloud.
- [[AWS IAM]]: un mal uso de IAM es una de las fuentes más frecuentes de problemas.
- [[Least Privilege]]: principio especialmente crítico en entornos cloud por la amplitud de los permisos disponibles.
