 - Modelo diseñado para el ámbito militar, focalizado en proteger la confidencialidad. 
 - Define que la información debe clasificarse de acuerdo al nivel de confidencialidad de la misma. 
 - Todos los sujetos deben tener un nivel de acceso definido, con los mismos posibles niveles de la información.

Los niveles de acceso comunes son:
**Top Secret**
**Secret**
**Confidential**
**Unclassified**

Para mantener la confidencialidad, el modelo de Bell-LaPadula impone dos reglas matemáticas estrictas que se resumen popularmente en dos frases:

> [!info] Propiedad de Seguridad Simple (Simple Security Property)
> **"No leer hacia arriba" (No Read Up)**
> Un sujeto con un nivel de seguridad determinado **solo puede leer** objetos de su propio nivel o de niveles inferiores.
> 
> - _Ejemplo:_ Si tienes acceso "Confidencial", puedes leer documentos "Confidenciales" o "Públicos", pero tienes estrictamente prohibido leer un documento "Top Secret".
    
>[!info] Propiedad de la Estrella ( $*$-Property / Star Property)
> **"No escribir hacia abajo" (No Write Down)**
> Un sujeto con un nivel de seguridad determinado **solo puede escribir** (modificar o crear) objetos de su propio nivel o de niveles superiores.
> 
>- _Ejemplo:_ Si un oficial con acceso "Top Secret" pudiera escribir en un archivo "Público", podría copiar accidental o maliciosamente información altamente clasificada en un lugar donde cualquiera la vería. Por lo tanto, el sistema le prohíbe "bajar" información.


Aunque Bell-LaPadula es excelente para proteger secretos, tiene dos grandes problemas:
1. **Ignora la Integridad:** Al modelo solo le importa que la información no se filtre. No le importa si un usuario de nivel bajo escribe información falsa en un archivo de nivel alto (lo que corrompería los datos). Para solucionar esto, más tarde se creó el modelo **Biba**, que funciona exactamente al revés.
    
2. **Es muy rígido:** En el mundo real, a veces un documento "Top Secret" necesita ser desclasificado (por ejemplo, para enviarlo a la prensa o a aliados). BLP no permite bajar la clasificación de un archivo de forma natural; se requieren mecanismos especiales fuera del modelo (como la figura de un administrador de confianza).