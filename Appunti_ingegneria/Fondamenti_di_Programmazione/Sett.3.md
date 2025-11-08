---
layout: page
title: FDP - Lezione Settimana 3
permalink: /fondamenti/settimana-3/
order: 3
---
### Conversioni di tipo implicite ed esplicite

Spesso, il C++ deve eseguire operazioni tra tipi di dato diversi (es. `int` e `double`).

- **Conversioni Implicite:** Avvengono automaticamente, senza che tu lo chieda. Il compilatore "promuove" il tipo più "piccolo" a quello più "grande" per evitare perdita di informazioni.
    
    - **Esempio:** `int i = 5; double d = 10.5;`
        
    - `double somma = i + d;`
        
    - Per calcolare `i + d`, il compilatore converte _temporaneamente_ `i` (5) in un `double` (5.0). L'operazione diventa `5.0 + 10.5`, e il risultato `15.5` viene salvato in `somma`.


- **Conversioni Esplicite (Casting):** Sei tu che forzi il compilatore a cambiare un tipo di dato, anche se questo potrebbe causare una *perdita di informazioni*.
    
    - **Esempio:** Vuoi troncare un numero decimale.
        
    - `double pi = 3.14159;`
        
    - `int pi_troncato = (int)pi;` // C-style cast
        
    - Ora `pi_troncato` vale **3**. La parte decimale è stata persa _intenzionalmente_.

---
###  Operatore di casting `static_cast`

Il "C-style cast" `(int)valore` è considerato obsoleto e pericoloso in C++. Il modo moderno e più sicuro è usare `static_cast`.

È un'istruzione esplicita che dice: "So che sto cambiando tipo, e so cosa sto facendo". È più facile da trovare nel codice e il compilatore esegue controlli migliori.

- **Sintassi:** `static_cast<NuovoTipo>(valore)`

**Esempio:**
C++
```cpp
double pi = 3.14159;
// Modo preferito in C++ per fare la conversione esplicita:
int pi_troncato = static_cast<int>(pi); // pi_troncato vale 3
```


---

### Operatori di assegnamento composti

Sono scorciatoie per un'operazione comune: `variabile = variabile operatore valore`.

|Operatore|Esempio|Equivalente a|
|---|---|---|
|`+=`|`x += 5;`|`x = x + 5;`|
|`-=`|`x -= 5;`|`x = x - 5;`|
|`*=`|`x *= 2;`|`x = x * 2;`|
|`/=`|`x /= 2;`|`x = x / 2;`|
|`%=`|`x %= 3;`|`x = x % 3;`|
|`&=`, `|=`,` ^=`|...|

---
## Incremento e decremento

In C++, gli **operatori di incremento e decremento** servono per aumentare o diminuire il valore di una variabile di 1. Esistono due forme: **prefisso** e **postfisso**. Ti spiego nel dettaglio.
#### **a) Prefisso:** 

#### **++variabile**

- Incrementa la variabile **prima** di usarne il valore.
```cpp
int a = 5;    // x = x + 1
int b = ++a; // a diventa 6, b = 6
```

#### **b) Postfisso:** 

#### **variabile++**

- Usa il valore della variabile **prima**, poi incrementa.
```cpp
int a = 5;     // x = x - 1
int b = a++; // b = 5, poi a diventa 6
```

- Stessa cosa avviene con il decremento solo che diminuisce il valore di 1.



---


## Tipo unsigned

- unsigned è un tipo di dato **intero senza segno**.
    
- Significa che **non può rappresentare numeri negativi**, solo 0 e numeri positivi.
    
- Spesso si usa per contatori, indici di array o valori che **non dovrebbero mai essere negativi**.


Esempi di dichiarazioni equivalenti:
```cpp
unsigned int a;     // intero senza segno
unsigned short b;   // short senza segno
unsigned long c;    // long senza segno
```
 Nota: unsigned da solo è equivalente a unsigned int.



### `unsigned` è un modificatore per i tipi interi (`int`, `char`, `long`, ecc.) 
che dice al compilatore che quella variabile **non può essere negativa**.

- **Come funziona:** Usa il bit che normalmente sarebbe riservato al segno (il MSB) come parte del numero. Questo **raddoppia la capacità positiva**.

- **Esempio (su 8 bit):**
    
    - `signed char`: Intervallo da -128 a +127.
        
    - `unsigned char`: Intervallo da **0 a 255**.

- **Letterali:** Per specificare che un numero è `unsigned`, si aggiunge una `u` (o `U`) alla fine.
    
    - `100` (è un `int` normale)
        
    - `100u` (è un `unsigned int`)

---
#### Osservazioni:

 se N è il numero di bit impiegati per rappresentare gli interi, i valori vanno da **0 a $\mathbf{0 \text{ a } 2^{N-1}}$**
	• Il tipo unsigned è utilizzato principalmente per operazioni a basso livello: 
	    – il contenuto di alcune celle di memoria non è visto come un valore numerico, ma come una configurazione di bit.

## Operatori bit a bit:

			| OR bit a bit

            & AND bit a bit

			^ OR esclusivo bit a bit

			~ complemento bit a bit

			<< traslazione a sinistra

			>> traslazione a destra

| a   | b   | a OR b | a AND b | a XOR b | NOT a | NOT b |
| --- | --- | ------ | ------- | ------- | ----- | ----- |
| 0   | 0   | 0      | 0       | 0       | 1     | 1     |
| 0   | 1   | 1      | 0       | 1       | 1     | 0     |
| 1   | 0   | 1      | 0       | 1       | 0     | 1     |
| 1   | 1   | 1      | 1       | 0       | 0     | 0     |
|     |     |        |         |         |       |       |

#### AND bit a bit:
![[Pasted image 20251008135646.png | 400]]
#### OR bit a bit :
![[Pasted image 20251008140035.png | 400]]
#### XOR bit a bit:
![[Pasted image 20251008140247.png | 400]]


---

## Ciclo While

- Il ciclo `while` è un **ciclo pre-condizionato**, cioè **controlla la condizione prima di eseguire il blocco di istruzioni**.  
- Finché la condizione è vera (`true`), il ciclo continua; appena diventa falsa (`false`), il ciclo termina.  

**Sintassi generale:**

```cpp
while (condizione) {
    // istruzioni da ripetere
}
```

- **condizione** → è un’espressione booleana (cioè che vale true o false)
- Finché la condizione è vera (true), il blocco viene eseguito
- Quando la condizione diventa falsa (false), il ciclo termina


ESEMPIO CONTARE DA 1 A 5:
```cpp
int main() {
    int i = 1;  // inizializzazione

    while (i <= 5) {   // condizione
        cout << i << endl;   // corpo del ciclo
        i++;  // incremento (per evitare ciclo infinito)
    }

    return 0;
}
```

ESEMPI ASTERISCHI IN VARI MODI: 
```cpp
int main()
{
    int n, i = 0;

    cout << "Quanti asterischi? " << endl;
    cin >> n;

    while (i < n)
    {
        cout << '*';
        i++;
    } // n conserva il valore iniziale

    cout << endl;
    return 0;
}

// OPPURE

    int n;

    cout << "Quanti asterischi? " << endl;
    cin >> n;

    while (n > 0)
    {
        cout << '*';
        n--;
    } // al termine, n vale 0

    cout << endl;
    return 0;
}

// OPPURE

    int n;

    cout << "Quanti asterischi? " << endl;
    cin >> n;

    while (n-- > 0)
        cout << '*'; // al termine, n vale -1

    cout << endl;
    return 0;
}
```

ES: Legge, raddoppia e scrive interi non negativi,
Termina al primo negativo
```cpp
int main()
{
    int i;

    cout << "Inserisci un numero intero" << endl;
    cin >> i;

    while (i >= 0)
    {
        cout << 2 * i << endl;
        cout << "Inserisci un numero intero" << endl;
        cin >> i;
    }

    return 0;
} 
Inserisci un numero intero
1
2
Inserisci un numero intero
3
6
Inserisci un numero intero
-2
```


## Maschera di bit

```cpp
unsigned int n;  
cout << "Inserisci n: ";  
cin >> n;  
  
// Maschera M1 con il bit n a 1 e tutti gli altri a 0  
unsigned short int M1 = (1 << n);  
  
// Stampa M1 in base 10  
cout << "Valore numerico della maschera M1: " << M1 << endl;  
  
// Stampa M1 in base 16  
cout << "Visualizzato in base 16: " << hex << M1 << endl;  
  
// Torna alla base 10  
cout << dec;  
  
// Maschera M2 con il bit n a 0 e tutti gli altri a 1  
unsigned short int M2 = ~(1 << n);  
  
// Stampa M2 in base 10  
cout << "Valore numerico della maschera M2: " << M2 << endl;  
  
// Stampa M2 in base 16  
cout << "Visualizzato in base 16: " << hex << M2 << endl;
```

```cpp
unsigned short int M1 = (1 << n);
``` 
Questa è la parte più importante.

- 1 << n significa **sposta il bit 1 a sinistra di n posizioni**.
    
- In pratica, crea un numero binario con **solo il bit n impostato a 1** e tutti gli altri a 0.
    

##### Esempio:

- Se n = 0 → M1 = 0b0000000000000001 → in decimale = **1**
    
- Se n = 3 → M1 = 0b0000000000001000 → in decimale = **8**
    
- Se n = 7 → M1 = 0b10000000 → in decimale = **128**

### ** 3. Stampa di**  M1 in base 10 e 16
```cpp
cout << "Valore numerico della maschera M1: " << M1 << endl;
cout << "Visualizzato in base 16: " << hex << M1 << endl;
```
- hex imposta la stampa in **base 16 (esadecimale)**
    
- dec serve per tornare alla **base 10 (decimale)**


##### Esempio:

- Se n = 4, M1 = 16 →
    
    In base 10: 16
    
    In base 16: 10

### **4. Creazione della maschera** M2
```cpp
unsigned short int M2 = ~(1 << n);
```
Qui invece fai **l’opposto** di prima:

1. 1 << n crea la maschera con **solo il bit n a 1**.
    
2. ~ (NOT bit a bit) **inverte tutti i bit** → il bit n diventa 0, e tutti gli altri diventano 1.

##### Esempio:

- Se n = 3:
    
    1 << n = 0b00001000
    
    ~(1 << n) = 0b11110111


Quindi M2 è una maschera con **tutti i bit a 1 tranne il bit n**.

### **5. Stampa di** M2 in base 10 e 16
```cpp
cout << "Valore numerico della maschera M2: " << M2 << endl;
cout << "Visualizzato in base 16: " << hex << M2 << endl;
```
- Se n = 3, M2 = 0b11110111
    
    → In decimale (su 16 bit) = **65527**
    
    → In esadecimale = **fff7**

### Riassunto concettuale

| **Maschera**   | **Significato**             | **Binario (se n=3)** | **Effetto**                                          |
| -------------- | --------------------------- | -------------------- | ---------------------------------------------------- |
| M1 = (1 << n)  | Solo il bit n è 1           | 00001000             | Serve per **attivare o controllare** il bit n        |
| M2 = ~(1 << n) | Tutti i bit tranne n sono 1 | 11110111             | Serve per **azzerare** il bit n mantenendo gli altri |
### **Esempio completo**
Se inserisci n = 4, il programma farà:
```cpp
M1 = 1 << 4 = 0b00010000 → 16 (dec) → 10 (hex)
M2 = ~(1 << 4) = 0b11101111 → 65519 (dec) → ffeF (hex)
```

---

## Codifica binaria 
Si usa   bitset <32>(x)
ESEMPIO:
```cpp
int x;  
cout << "Digita un numero senza segno: ";  
cin >> x;  
  
cout << "La codifica binaria di " << x << " è: " << bitset<32>(x) << endl;
```





---
---



# TEORIA
### Rappresentazione dei numeri interi con **bias** (o Eccesso N)

È un altro modo (diverso da M&S e C2) per rappresentare numeri positivi e negativi. È usato internamente nei processori, specialmente per gli esponenti dei numeri in virgola mobile.

- **Concetto:** Si sceglie un numero fisso detto **Bias** (o Eccesso). La rappresentazione binaria non memorizza il valore vero, ma `Valore + Bias`.

- **Il Bias:** Di solito si sceglie $B = 2^{(n−1)}$ (dove n è il numero di bit).
    
    - Su 4 bit (n=4), il Bias è $2^3=8$. (Si dice "Eccesso 8").

---
#### Algoritmi (Esempio: 4 bit, Bias = 8)

- **Codifica (da Valore a Bias):** `Rappresentazione = Valore + Bias`
    
    1. **Valore = 3**: `3 + 8 = 11`. Converti 11 in binario: `1011`
        
    2. **Valore = -5**: `-5 + 8 = 3`. Converti 3 in binario: `0011`
        
    3. **Valore = 0**: `0 + 8 = 8`. Converti 8 in binario: `1000`



- **Decodifica (da Bias a Valore):** `Valore = Rappresentazione - Bias`
    
    1. **Binario = `1110`**: Converti 1110 (unsigned) → 14. `Valore = 14 - 8 = +6`
        
    2. **Binario = `0101`**: Converti 0101 (unsigned) → 5. `Valore = 5 - 8 = -3`


- **Intervallo (su 4 bit, Bias=8):**
    
    - La rappresentazione più piccola è `0000` (vale 0) → `0 - 8 = -8`
        
    - La rappresentazione più grande è `1111` (vale 15) → `15 - 8 = +7`
        
    - **Intervallo: da -8 a +7**.

---

###  Rappresentazione dei numeri reali in **virgola fissa**

È un metodo per rappresentare numeri con la virgola (es. 3.25) usando solo la matematica degli interi. È utile in sistemi senza un'unità di calcolo per la virgola mobile (FPU), come i microcontrollori.

- **Concetto:** Si prende un intero (es. 16 bit) e si **decide** (a livello di software) che la virgola si trova in una posizione fissa.

- **Esempio (Formato Q8.8):**
    
    - Usi 16 bit.
        
    - I primi 8 bit sono la **parte intera**.
        
    - Gli ultimi 8 bit sono la **parte frazionaria**.



- **Come leggere il numero `0000 0101.1100 0000` (Q8.8):**
    
    1. **Parte Intera (sinistra):** `0000 0101` → 5
        
    2. **Parte Frazionaria (destra):** `1100 0000` → $1⋅2^{−1} + 1⋅2^{−2}+0⋅$...
        
        - 1⋅0.5+1⋅0.25=0.75
            
    3. **Valore Totale:** 5.75


---

