Son entidades que centralizan intercambio de claves

Comparten una clave con cada entidad que participa $(k_{a},k_{b},k_{c},\dots)$
Si $A$ quiere comunicarse con $C$, envía un pedido al KDC, el KDC crea una nueva clave de sesión $k_{s}$, y se la envía a A y C, cifrando con $k_{a}$ y $k_{c}$ respectivamente