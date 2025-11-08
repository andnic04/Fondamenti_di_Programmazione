---
layout: default
title: Settimana 2 - Variabili e Tipi
parent: Fondamenti di Programmazione  
nav_order: 2                        
---
## Strutture di Controllo e Operatori (C++)

Questa sezione riguarda le fondamenta della scrittura di codice: come è strutturato un programma e come prende decisioni.

### Struttura di un programma C/C++

Ogni programma C++ ha una struttura di base. Pensa a un "tema" scolastico: ha un'intestazione, un corpo centrale e una conclusione.

- **Direttive al pre-processore:** Istruzioni che iniziano con `#`, come `#include <iostream>`. Dicono al compilatore di includere librerie esterne (in questo caso, per l'input/output).

- **Funzione `main`:** È il **punto d'inizio** obbligatorio. Il tuo programma inizia sempre eseguendo il codice dentro `int main() { ... }`.

- **Corpo della funzione:** Racchiuso tra parentesi graffe `{ ... }`.

- **Istruzione di `return`:** `return 0;` segnala al sistema operativo che il programma è terminato senza errori.


**Esempio minimo:**

C++
```cpp
#include <iostream>        // 1. Direttiva al preprocessore

int main() {               // 2. Punto d'inizio: la funzione main
    cout << "Ciao, mondo!" << endl;    // 3. Corpo: istruzioni
    
    return 0;              // 4. Conclusione
}
```

---
### Teorema di Bohm-Jacopini
^fcf509

- **Teorema di Bohm-Jacopini (1966):** Questo teorema è il fondamento teorico della programmazione strutturata. Dimostra che **qualsiasi algoritmo** (non importa quanto complesso) può essere implementato usando solo tre strutture di controllo:

    1. **Sequenza:** Eseguire le istruzioni una dopo l'altra (è quello che succede normalmente).
        
    2. **Selezione:** Prendere una decisione e scegliere quale codice eseguire (es. `if-else`).
        
    3. **Iterazione (o Ciclo):** Ripetere un blocco di codice finché una condizione è vera (es. `while`, `for`).

---
### Operatori di confronto e logici

Gli operatori sono simboli che eseguono operazioni. Questi operatori producono sempre un valore booleano: **`true`** (vero) o **`false`** (falso).

- **Operatori di Confronto (Relazionali):**
    
    - `==` (Uguale a): `5 == 5` è `true`.
        
    - `!=` (Diverso da): `5 != 3` è `true`.
        
    - `<` (Minore di): `3 < 5` è `true`.
        
    - `>` (Maggiore di): `3 > 5` è `false`.
        
    - `<=` (Minore o uguale a): `5 <= 5` è `true`.
        
    - `>=` (Maggiore o uguale a): `5 >= 6` è `false`.

- **Operatori Logici:** Combinano valori `true`/`false`.
    
    - `&&` (AND logico): È `true` solo se **entrambi** gli operandi sono `true`.
        
        - `Esempio: (eta >= 18) && (ha_patente == true)`
            
    - `||` (OR logico): È `true` se **almeno uno** degli operandi è `true`.
        
        - `Esempio: (giorno == "sabato") || (giorno == "domenica")`
            
    - `!` (NOT logico): **Inverte** il valore.
        
        - `Esempio: !(is_open)` è `true` se `is_open` è `false`.

---
###  Istruzione di selezione `if` e `if-else`

È la struttura di **selezione** fondamentale. Permette al programma di scegliere cosa fare.

- **`if` (semplice):** Esegue il blocco solo se la condizione è vera.
C++
```cpp
int temperatura = 30;
	if (temperatura > 25) {
	    cout << "Fa caldo!" << endl;
}
```


- **`if-else` (a due vie):** Esegue un blocco se la condizione è vera, altrimenti ne esegue un altro.
C++
```cpp
int voto = 25;
	if (voto >= 18) {
	    cout << "Promosso." << endl;
} 
	else {
	    cout << "Bocciato." << endl;
}
```

---
### Operatore condizionale (ternario)

È una scorciatoia per un semplice `if-else` che assegna un valore a una variabile. La sua sintassi è: 
	`condizione ? valore_se_vero : valore_se_falso`

**Esempio:**
C++
```cpp
int voto = 25;
cout << ((voto >= 18) ? "Promosso." : "Bocciato.") << endl;
```

---
### I cinque aspetti degli operatori

Ogni operatore è definito da 5 caratteristiche:

1. **Simbolo:** Come appare (es. `+`, `*`, `==`, `&`).

2. **Arietà:** Quanti operandi richiede.

    - **Unario:** 1 operando (es. `-5`, `!vero`, `x++`).
        
    - **Binario:** 2 operandi (es. `x + y`, `a == b`).
        
    - **Ternario:** 3 operandi (l'unico è l'operatore condizionale `?:`).

3. **Posizione:** Dove si trova rispetto agli operandi.
    
    - **Prefisso:** Prima dell'operando (es. `++x`, `-5`).
        
    - **Infisso:** Tra gli operandi (es. `x + y`). È la più comune.
        
    - **Postfisso:** Dopo l'operando (es. `x++`).

4. **Associatività:** Come si raggruppano gli operatori con la _stessa priorità_.
    
    - **Da sinistra a destra:** `a - b - c` viene valutato come `(a - b) - c`. (Comune per l'aritmetica).
        
    - **Da destra a sinistra:** `a = b = c` viene valutato come `a = (b = c)`. (Comune per l'assegnamento).

5. **Priorità (o Precedenza):** Quale operatore viene eseguito per primo.
    
    - **Esempio:** In `2 + 3 * 4`, l'operatore `*` (moltiplicazione) ha priorità più alta del `+` (addizione), quindi viene eseguito prima: `2 + (3 * 4) = 14`.

---
### Operatori binari e unari

- **Binario:** Prende due operandi.
    - `a + b`
        
    - `x > y`
        
    - `c = 10` (l'assegnamento `=` è un operatore binario)

- **Unario:** Prende un solo operando.
    - `-10` (meno unario)
        
    - `!is_ready` (NOT logico)
        
    - `++contatore` (incremento prefisso)
---
### Trasformazione di espressioni in istruzioni (expression statement)

Per trasformare un'espressione in un'istruzione, basta aggiungergli un punto e virgola `;`. 
Il valore prodotto dall'espressione viene "scartato".

**Esempi:**

- `x = 10` (espressione) -> `x = 10;` (istruzione: assegna 10 a x)
    
- `i++` (espressione) -> `i++;` (istruzione: incrementa i)
    
- `2 + 3` (espressione) -> `2 + 3;` (istruzione: calcola 5 e... buttalo via. È un'istruzione valida ma inutile!)
---

# TIPI FONDAMENTALI


### -Il tipo `CHAR` e i suoi letterali

- **Cos'è:** Il tipo `char` è usato per memorizzare ==**un singolo carattere**==. In C++, è tecnicamente un tipo intero molto piccolo (di solito 1 byte = 8 bit).

- **Perché intero?** Perché ogni carattere è mappato a un numero intero secondo una tabella standard, la più famosa è la **ASCII**.

    - Esempio ASCII: `'A'` corrisponde al numero 65, `'B'` a 66, `'a'` a 97, `'0'` al numero 48.


- **Letterali `char`:** Si scrivono tra **apici singoli `' '`**.
    
    - `'a'`
        
    - `'7'` (questo è il _carattere_ '7', non il _numero_ 7)
        
    - `'$'`

    - **Caratteri speciali (escape):**
        
        - `'\n'` (a capo - newline)
            
        - `'\t'` (tabulazione - tab)
            
        - `'\\'` (il carattere backslash)
            
        - `'\''` (il carattere apice singolo)



### Operazioni definite sul tipo `char`

Dato che `char` è un tipo intero, puoi farci (con cautela) l'aritmetica.

1. **Confronto:** Puoi confrontare i caratteri. Il confronto avviene sui loro codici ASCII.
    
    - `'a' < 'b'` è `true` (perché 97 < 98)
        
    - `'A' < 'a'` è `true` (perché 65 < 97)

2. **Aritmetica:** Puoi sommare e sottrarre.
    
    - `'A' + 1` dà come risultato `'B'` (perché 65 + 1 = 66).
        
    - **Esempio utile:** Convertire un carattere maiuscolo nel rispettivo minuscolo (in ASCII):

C++
```cpp
char maiuscola = 'C';
char minuscola = maiuscola + 32; // In ASCII la differenza è 32
// Oppure, in modo più portabile:
char minuscola_portabile = maiuscola + ('a' - 'A'); 
// minuscola_portabile ora contiene 'c'
```


---
---



## TEORIA:  Rappresentazione Numeri Interi


####  Rappresentazione dei numeri interi (in base 2)

Il computer non capisce il sistema decimale (base 10). Capisce solo il **sistema binario (base 2)**, basato su due cifre: 0 e 1 (chiamate **bit**).

- **Decimale (Base 10):** Il numero `125` significa **$1⋅10^2 + 2⋅10^1 + 5⋅10^0.$**

- **Binario (Base 2):** Il numero `1101` significa $1⋅2^3+1⋅2^2+0⋅2^1+1⋅2^0$ = 8+4+0+1 = 13 (in decimale).

Per rappresentare i numeri _negativi_ servono delle convenzioni. Le due principali sono Modulo e Segno e Complemento a Due.

---
###  Rappresentazione in Modulo e Segno (M&S)

È il modo più intuitivo. Si usa un numero fisso di bit (es. 8 bit).

- Il **primo bit** (quello più a sinistra, o MSB - _Most Significant Bit_) è il **bit di segno**:

    - **0** = Positivo

    - **1** = Negativo

- Gli **altri (n-1) bit** rappresentano il **valore assoluto** (modulo) del numero.


**Esempio (su 4 bit):**

- `+5`: 5 in binario è `101`. Su 3 bit è `101`. Il segno è 0. Risultato: `0101`

- `-5`: 5 in binario è `101`. Su 3 bit è `101`. Il segno è 1. Risultato: `1101`

- `+0`: `0000`

- `-0`: `1000`


**Svantaggi:**

1. **Doppio zero:** Esistono due rappresentazioni per lo zero (`+0` e `-0`), il che è uno spreco e crea problemi.

2. **Aritmetica complessa:** Sommare un numero positivo e uno negativo richiede circuiti diversi rispetto a sommare due positivi.

---

### Rappresentazione in Complemento a Due (C2)

È lo standard usato da quasi tutti i computer moderni perché risolve i problemi del M&S.

- **Numeri Positivi:** Si rappresentano esattamente come in M&S. Il primo bit (MSB) è **0**.
    
    - `+5` (su 4 bit) è `0101`.

- **Numeri Negativi:** La rappresentazione è diversa e il primo bit (MSB) è **1**.


**Vantaggi:**

1. **Un solo zero:** `0000` è l'unica rappresentazione per lo zero.
    
2. **Aritmetica semplice:** La somma algebrica funziona "magicamente" usando gli stessi circuiti della somma tra positivi. (Es. `5 + (-5)` in binario dà `0000`).

---
### Algoritmi di codifica e decodifica

Assumiamo di lavorare su **n** bit (es. n=8).

#### Modulo e Segno (M&S)

- **Codifica (da Decimale a M&S):** Es. "Codifica -25 su 8 bit"
    
    1. Converti il modulo (25) in binario: `11001`.
        
    2. Riempi con zeri a sinistra (padding) per occupare n-1 bit (7 bit): `0011001`.
        
    3. Aggiungi il bit di segno: `1` perché è negativo.
        
    4. **Risultato:** `10011001`


- **Decodifica (da M&S a Decimale):** Es. "Decodifica `10101100`"
    
    1. Guarda il bit di segno (MSB): è `1`, quindi il numero è **negativo**.
        
    2. Converti i restanti n-1 bit (modulo): `0101100` -> 	$$0·2^6 + 1·2^5 + 0·2^4 + 1·2^3 + 1·2^2 + 0·2^1 + 0·2^0 = 0+32+0+8+4+0+0= 44$$
    3. **Risultato:** -44

---

#### Complemento a Due (C2)

- **Codifica (da Decimale a C2):** Es. "Codifica -25 su 8 bit"
    
    1. Parti dal numero **positivo** (+25) e rappresentalo su n bit: `00011001`.
        
    2. **Inverti tutti i bit** (complemento a uno): `11100110`.
        
    3. **Somma 1** al risultato: `11100110 + 1`.
        
    4. **Risultato:** `11100111`

- **Decodifica (da C2 a Decimale):** Es. "Decodifica `11100111`"
    
    - **Metodo 1 (Pesi con MSB negativo):** Il MSB (bit n-1) non vale +2n−1 ma **−2n−1**.
        
        - `11100111` (su 8 bit):
            
        - $−1⋅2^7+1⋅2^6+1⋅2^5+0⋅2^4+0⋅2^3+1⋅2^2+1⋅2^1+1⋅2^0$
            
        - −128+64+32+0+0+4+2+1 = −128 + 103 = −25.
            
        - **Risultato:** -25


    - **Metodo 2 (Inversione):** Se vedi che inizia per `1`, sai che è negativo. Per trovare il valore assoluto, rifai l'algoritmo (inverti + somma 1).

        1. Numero: `11100111` (è negativo).
            
        2. **Sottrai 1**: `11100110`.
            
        3. **Inverti i bit**: `00011001`.
            
        4. Converti questo risultato (che è il modulo): `16 + 8 + 1 = 25`.
            
        5. **Risultato:** -25

```
  11100111
- 00000001  <-- (Questo è '1' in 8 bit)
----------
  11100110
```

---

### Intervalli di rappresentabilità

Dato un numero **n** di bit (es. 8 bit):

- **Senza segno (unsigned):**
    
    - Puoi rappresentare 2n numeri.
        
    - Intervallo: da 0 a 2n−1.
        
    - _Esempio (8 bit):_ da 0 a 255.

- **Modulo e Segno:**
    
    - Usi 1 bit per il segno e n-1 per il modulo.
        
    - Intervallo: da −(2(n−1)−1) a +(2(n−1)−1).
        
    - _Esempio (8 bit):_ da -127 a +127 (con due zeri).

- **Complemento a Due:**
    
    - Risolve il problema del doppio zero "usando" la rappresentazione `1000...` per un negativo in più.
        
    - Intervallo: da −2(n−1) a +(2(n−1)−1).
        
    - _Esempio (8 bit):_ da **-128** a **+127**. (È asimmetrico!)


---

## Pratica: Esercitazione e Laboratorio


### Esercizi sulla rappresentazione dei numeri

**Esercizio Tipo 1:**

> Rappresentare il numero **-42** (decimale) su **8 bit** in Modulo e Segno (M&S) e in Complemento a Due (C2).

**Soluzione M&S:**

1. Modulo: 42 (dec) = `101010` (bin)
    
2. Padding a 7 bit: `0101010`
    
3. Bit di segno (1): `10101010`
    
4. **Risultato (M&S): `10101010`**


**Soluzione C2:**

1. Positivo +42 (su 8 bit): `00101010`
    
2. Inverti i bit: `11010101`
    
3. Somma 1: `11010101 + 1 = 11010110`
    
4. **Risultato (C2): `11010110`**




**Esercizio Tipo 2:**

> Dato il numero binario su 8 bit `10010011`, quale valore decimale rappresenta se codificato in M&S? E se in C2?

**Soluzione M&S:**

1. Segno: `1` (negativo).
    
2. Modulo: `0010011` (bin) = 16+2+1=19.
    
3. **Risultato (M&S): -19**
    

**Soluzione C2:**

1. Segno: `1` (negativo). Uso il metodo dei pesi.
    
2. Calcolo: −128+(16+2+1)=−128+19=−109.
    
3. **Risultato (C2): -109**


---