- Creado por David D. Clark y David R. Wilson. 
- Modelo diseñado para proteger la integridad de la información. 
- Define que los datos pueden estar restringidos (protegidos) o sin restricción. 
- Los datos restringidos sólo pueden ser modificados por medio de un proceso de transformación (TP). 
- Es responsabilidad de los procesos de transformación validar la integridad de la información.
- Los sujetos sólo modifican los datos restringidos por medio de los procesos de transformación. 
- Incorpora el concepto de separation of duty. Quién modifica los datos no puede ser el mismo que verifica la integridad

> [!info] Los Componentes de Clark-Wilson
> A diferencia de los modelos anteriores que solo hablan de "sujetos" y "objetos", Clark-Wilson introduce una estructura más madura dividida en cuatro elementos:
> 1. **CDI (Constrained Data Items / Datos Restringidos):** Son los datos súper importantes que el modelo debe proteger (por ejemplo, el saldo de una cuenta bancaria o el inventario de una empresa).
>     
> 2. **UDI (Unconstrained Data Items / Datos No Restringidos):** Son datos externos o de baja importancia que no necesitan pasar por controles estrictos (por ejemplo, el cuadro de texto donde un usuario escribe una queja).
>     
> 3. **TP (Transformation Procedures / Procedimientos de Transformación):** Son los **únicos programas o funciones autorizados** para modificar los datos importantes (CDIs). Un usuario nunca modifica el saldo directamente; usa el TP "HacerTransferencia()".
>     
> 4. **IVP (Integrity Verification Procedures / Procedimientos de Verificación de Integridad):** Son programas que corren de fondo para revisar que los datos estén correctos y cuadren (por ejemplo, una auditoría que verifique que la suma de todos los gastos coincida con el dinero que salió del banco).

### Diferencias con [[Modelo Biba]]

| Caracteristica         | Biba                                                                       | Clark-Wilson                                                             |
| ---------------------- | -------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| **Enfoque**            | Basado en **Niveles de Sujeto/Objeto** (Matemático).                       | Basado en **Procesos y Roles** (Comercial).                              |
| **Acceso a los Datos** | El usuario accede **directamente** al objeto (si su nivel se lo permite).  | El usuario **nunca** accede directo; usa un programa intermediario (TP). |
| **Mecanismo Central**  | Reglas rígidas de flujo (No leer abajo, no escribir arriba).               | Transacciones bien formadas y separación de funciones.                   |
| **Control de Fraude**  | No lo previene eficazmente (un usuario con nivel alto puede hacer fraude). | Lo previene mediante la separación de tareas (hacen falta dos personas). |
| **Ideal para...**      | Sistemas operativos, firmware, desarrollo de software seguro.              | Aplicaciones de negocio, banca, ERPs (como SAP), contabilidad.           |