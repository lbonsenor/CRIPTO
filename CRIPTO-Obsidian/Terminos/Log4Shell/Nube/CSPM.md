**CSPM** (_Cloud Security Posture Management_) es una categoría de herramientas que buscan y corrigen **problemas de configuración y permisos** en entornos cloud.

## Funciones principales

- **Analizar IaC** (_Infrastructure as Code_) antes de que sea desplegado, para detectar configuraciones incorrectas antes de que lleguen a producción.
- **Revisar la configuración actual** del entorno cloud para detectar cambios o desviaciones respecto a la configuración esperada (_configuration drift_).
- Buscar **permisos excesivos** y recursos expuestos públicamente sin justificación.

## Enfoque

El CSPM tiene un enfoque principalmente **estático**: analiza el estado de la configuración, no el comportamiento en ejecución. Para el comportamiento dinámico, se complementa con [[CWPP]].

## Ejemplos de problemas que detecta

- Buckets S3 con acceso público sin intención.
- Grupos de seguridad con puertos innecesariamente abiertos.
- Claves de API hardcodeadas en IaC.
- Roles IAM con permisos excesivos.

## Relación con otros conceptos

- [[CWPP]]: complemento con enfoque dinámico en las aplicaciones en ejecución.
- [[CIEM]]: complemento enfocado en permisos y entitlements.
- [[CNAPP]]: plataforma que integra CSPM, CWPP y CIEM.
- [[Cloud Vulnerabilities]]: las malas configuraciones que CSPM detecta.
- [[AWS IAM]]: uno de los sistemas que CSPM audita.
- [[Least Privilege]]: principio que CSPM ayuda a verificar.
