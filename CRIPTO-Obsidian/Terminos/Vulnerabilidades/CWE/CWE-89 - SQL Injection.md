**CWE-89** representa la falta de control en el ingreso de datos del usuario que permite la **ejecución de código SQL arbitrario** en la base de datos.

## Descripción

La aplicación construye consultas SQL concatenando directamente el input del usuario, sin sanitizarlo ni usar consultas parametrizadas. Un atacante puede inyectar fragmentos SQL que alteran la lógica de la consulta.

## Consecuencias

- Extracción de datos arbitrarios de la base de datos (bypass de autorización).
- Modificación o eliminación de datos.
- En algunos casos: ejecución de comandos del sistema operativo (via `xp_cmdshell` en SQL Server, `COPY TO/FROM PROGRAM` en PostgreSQL).
- Bypass de autenticación.

## Ejemplo conceptual

```sql
-- Consulta vulnerable
SELECT * FROM users WHERE user='$input' AND pass='$pass';

-- Input del atacante: ' OR '1'='1
-- Consulta resultante (siempre verdadera):
SELECT * FROM users WHERE user='' OR '1'='1' AND pass='';
```

## Prevención

- Usar **consultas parametrizadas** (_prepared statements_) siempre.
- Nunca concatenar input del usuario directamente en SQL.
- Aplicar [[Least Privilege]] a la cuenta de base de datos de la aplicación.
- Validar y sanitizar todos los inputs.

## Relación con otros conceptos

- [[PostgreSQL - Roles]]: aplicar mínimo privilegio al usuario de la app mitiga el daño.
- [[Least Privilege]]: principio de diseño que limita el impacto.

## Referencia

https://cwe.mitre.org/data/definitions/89.html
