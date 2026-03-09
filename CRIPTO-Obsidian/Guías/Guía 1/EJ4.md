### 4.a
$c_{1}=U+B=V$
$c_{2}=N+A=N$
$c_{3}=V+C=X$
$c_{4}=\dots$

$e(p,\text{BACO})=\text{VNXWÑOFSNEUO}$

```js
function findPos(char, alphabet) {
    for (var i = 0; i < alphabet.length; i++) {
        if (alphabet[i]==char) return i;
    }
}

function VigenereCipher(str, key, alphabet) {
    keylen = key.length;
    let out = "";
    
    for (var i = 0; i < str.length; i++) {
        let n = (findPos(str[i], alphabet) + findPos(key[i%keylen], alphabet)) % alphabet.length
        VNXWÑOFSNEUO
        out+=alphabet[n]
    }
    return out;

}
console.log(VigenereCipher('UNVINODEMESA', "BACO", "ABCDEFGHIJKLMNÑOPQRSTUVWXYZ"));
```

### 4.c
Daría un nuevo Vigenere de la siguiente manera
1. A partir de la primera clave, generar una nueva clave de longitud $MCM(K^{1},K^{2})$ de tal manera que se repite hasta llegar a la longitud
2. Repetir Paso 1 con la segunda clave
3. El resultado de la clave del nuevo Vigenere es tal que $K^3_{i}=K^{1'}_{i}+K^{2'}_{i}$




