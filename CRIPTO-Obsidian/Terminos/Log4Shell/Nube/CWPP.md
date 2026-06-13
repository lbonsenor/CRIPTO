**CWPP** (_Cloud Workload Protection Platform_) se focaliza en la **protección de las aplicaciones corriendo en la nube**, con un enfoque dinámico y en tiempo de ejecución.

## Alcance

Cubre todos los tipos de workloads cloud:

- Servidores virtuales (VMs)
- [[Contenedores]]
- Funciones serverless

## Funciones principales

- **Análisis de comportamiento**: detecta actividad anómala en tiempo de ejecución (ej: un contenedor que empieza a hacer llamadas de red inesperadas).
- **Microsegmentación** a nivel de contenedores: limita la comunicación entre workloads al mínimo necesario ([[Least Privilege]] a nivel de red).
- **Threat intelligence**: integra inteligencia sobre amenazas para detectar patrones conocidos.

## Diferencia con CSPM

| Aspecto | [[CSPM]] | CWPP |
|---|---|---|
| Enfoque | Estático — configuración e IaC | Dinámico — comportamiento en ejecución |
| Qué protege | Configuración del entorno | Aplicaciones corriendo |
| Cuándo actúa | Antes del despliegue y en auditoría | Durante la ejecución |

Ambos se complementan: CSPM previene configuraciones incorrectas, CWPP detecta comportamientos maliciosos en lo que ya está corriendo.

## Relación con otros conceptos

- [[CSPM]]: complemento de enfoque estático.
- [[CIEM]]: complemento de enfoque en permisos.
- [[CNAPP]]: plataforma que integra CSPM, CWPP y CIEM.
- [[Contenedores]]: uno de los workloads que CWPP protege.
- [[Sandboxing]]: CWPP implementa sandboxing a nivel de workload cloud.
- [[Malware Attack Cycle]]: CWPP detecta comportamientos correspondientes a fases del ciclo de ataque.
