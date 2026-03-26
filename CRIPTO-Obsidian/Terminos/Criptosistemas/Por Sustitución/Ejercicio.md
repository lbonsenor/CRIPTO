### Enunciado
**Descifrar el siguiente mensaje con las siguientes ayudas**:
- El mensaje original está en castellano. 
- La separación en grupos de 5 símbolos no es parte del problema, simplemente ayuda a contar mejor.  
- Gancho: “LACABEZA” (aparece en el mensaje plano)

```
♦>]♣0 ♥34♦3 44]>} >{:☺♠ 
{4♥3> 34]02 014:0 34(♠4 
♥♣>♠) 0]{)4 03401 4}034 
♦344] 0♥1>]
```

### Solución
La tabla de frecuencias del idioma español se puede representar de la siguiente manera:

| Frec. Alta |         | Frec. Media |        | Frec. Baja |        |
| ---------- | ------- | ----------- | ------ | ---------- | ------ |
| E          | $13.11$ | C           | $4.85$ | Y          | $0.79$ |
| A          | $10.60$ | L           | $4.42$ | Q          | $0.74$ |
| S          | $8.47$  | U           | $4.34$ | H          | $0.60$ |
| O          | $8.23$  | M           | $3.11$ | Z          | $0.26$ |
| I          | $7.16$  | P           | $2.71$ | J          | $0.25$ |
| N          | $7.14$  | G           | $1.40$ | X          | $0.15$ |
| R          | $6.95$  | B           | $1.16$ | W          | $0.12$ |
| D          | $5.87$  | F           | $1.13$ | K          | $0.11$ |
| T          | $5.40$  | V           | $0.82$ | Ñ          | $0.10$ |

La frecuencia de caracteres del texto dado es la siguiente:

| Character | Count | Percentage |
| --------- | ----- | ---------- |
| 4         | 14    | 20.00%     |
| 0         | 9     | 12.86%     |
| 3         | 8     | 11.43%     |
| >         | 6     | 8.57%      |
| ]         | 6     | 8.57%      |
| ♥         | 4     | 5.71%      |
| ♦         | 3     | 4.29%      |
| {         | 3     | 4.29%      |
| ♠         | 3     | 4.29%      |
| 1         | 3     | 4.29%      |
| ♣         | 2     | 2.86%      |
| }         | 2     | 2.86%      |
| :         | 2     | 2.86%      |
| )         | 2     | 2.86%      |
| ☺         | 1     | 1.43%      |
| 2         | 1     | 1.43%      |
| (         | 1     | 1.43%      |

Asumamos que $d(40,k)=\text{EA}$
```
♦>]♣A ♥3E♦3 EE]>} >{:☺♠ {E♥3> 3E]A2 A1E:A 3E(♠E ♥♣>♠) A]{)E A3EA1 E}A3E ♦3EE] A♥1>]
```
Entonces la unica forma en la que esto sea correcto, por `A2 A1E:A`
$$d(\text{]02 014:0},k)=LACABEZA$$
```
♦>L♣A ♥3E♦3 EEL>} >{Z☺♠ {E♥3> 3ELAC ABEZA 3E(♠E ♥♣>♠) AL{)E A3EAB E}A3E ♦3EEL A♥B>L
```

Por palabras conocidas, podemos asumir que `A♥B>L` se refiere a ÁRBOL, por lo tanto

```
♦OL♣A R3E♦3 EELO} O{Z☺♠ {ER3O 3ELAC ABEZA 3E(♠E R♣O♠) AL{)E A3EAB E}A3E ♦3EEL ARBOL
```
Por palabras conocidas, podemos asumir que el símbolo `3` descifra la letra $\text{D}$

```
♦OL♣A RDE♦D EELO} O{Z☺♠ {ERDO DELAC ABEZA DE(♠E R♣O♠) AL{)E ADEAB E}ADE ♦DEEL ARBOL
```

Asumamos que 
$$d(\text{>\}>\{:☺♠\{4♥3>},k)=\text{OJOIZQUIERDO}$$
```
♦OL♣A RDE♦D EELOJ OIZQU IERDO DELAC ABEZA DE(UE R♣OU) ALI)E ADEAB EJADE ♦DEEL ARBOL
```

Reemplazando `♦` por $S$ y `♣` por $T$
``
```
SOLTA RDESD EELOJ OIZQU IERDO DELAC ABEZA DE(UE RTOU) ALI)E ADEAB EJADE SDEEL ARBOL
```

`(` por $M$ y `)` por $N$ nos da

```
SOLTA RDESD EELOJ OIZQU IERDO DELAC ABEZA DEMUE RTOUN ALINE ADEAB EJADE SDEEL ARBOL
```

Que da como resultado
> "Soltar desde el ojo izquierdo de la cabeza de muerto una linea de abeja desde el arbol"