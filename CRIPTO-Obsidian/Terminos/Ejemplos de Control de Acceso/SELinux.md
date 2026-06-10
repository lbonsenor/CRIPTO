**Security-Enhanced Linux (SELinux)** es un control de seguridad adicional al [[Discretionary Access Control]] aplicado en Linux por defecto (en distribuciones como Red Hat/CentOS/Fedora).

## Características

- Se basa en reglas para responder: **¿Puede el sujeto acceder al objeto?**
- Implementa un [[Mandatory Access Control]] sobre Linux, complementando el DAC estándar.
- Aplica el principio de **mínimo privilegio** a nivel de procesos.

## Contextos de SELinux

- Los **procesos** poseen un _SELinux context_.
- Todos los **recursos** (archivos, puertos, directorios) que un proceso puede acceder también tienen un contexto definido.

Ejemplos de contextos:

| Recurso | Contexto |
|---|---|
| `/var/tmp` | `t_var_tmp` |
| Puerto 80 | `t_port_www` |
| `/var/www/html` | `t_web_html` |

## Macros

Se utilizan **macros** para definir los permisos más comunes. El resultado del macro es un conjunto de reglas:

```
# macro-expander "mysql_read_config(httpd_t)"
allow httpd_t mysqld_etc_t : dir   { getattr search open read lock ioctl };
allow httpd_t mysqld_etc_t : file  { open { getattr read ioctl lock } };
allow httpd_t mysqld_etc_t : lnk_file { getattr read };
```

## Aislamiento entre procesos con mismo UID

Dos procesos corriendo con `UID 0` (root) pueden tener restringidos los accesos a los recursos del otro gracias a SELinux. Esto va más allá de lo que permite el DAC tradicional.

## Relación con otros conceptos

- [[Mandatory Access Control]]: SELinux es una implementación de MAC en Linux.
- [[RuBAC]]: las reglas de SELinux son una forma de control basado en reglas.
- [[Discretionary Access Control]]: SELinux lo complementa, no lo reemplaza.

## Referencia

https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/using_selinux/
