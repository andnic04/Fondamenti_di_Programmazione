---
layout: default
title: Settimana 7 - Matrici
parent: Fondamenti di Programmazione
nav_order: 7
---

# MATRICI (Array Multidimensionali)

Un array multidimensionale in C++ non è altro che un "array di array". Il caso più comune è l'array a due dimensioni (2D), che puoi immaginare come una tabella o una matrice con righe e colonne. Sono tipi di array i cui elementi sono array a loro volta: un array del primo tipo si dice monodimensionale, mentre uno del secondo tipo si dice multidimensionale. Per analogia col linguaggio matematico, gli array monodimensionale si chiamano anche vettori. 

### 1. Cosa Sono e Come si Dichiarano 📐

Un array multidimensionale è memorizzato in modo contiguo in memoria. 
Un array 2D `int matrice[3][4]` (3 righe e 4 colonne) è semplicemente un blocco unico di 12 interi. Il compilatore li gestisce come 3 array consecutivi, ognuno contenente 4 interi.

#### Dichiarazione

La sintassi generale è: `tipo nome[dimensione1][dimensione2]...[dimensioneN];`

- **Array 2D (Matrice):**
```cpp
// Una matrice di 3 righe e 4 colonne
int matrice[3][4];

matrice[0][1];   //0 individua la prima riga (di indice 0), dalla quale
                 //viene selezionato il secondo elemento (di indice 1)
```
Si tratta di un array di 3 elementi (righe), ciascuno costituito da un array di 4 interi (colonne)

#### MEMORIZZAZIONE: 
```cpp
int m[2][3] = {1, 2, 3, 4, 5, 6};

//viene memorizzato in questo ordine:
cout << m[0][0] = 1   //stampa 1
m[0][1] = 2
m[0][2] = 3
m[1][0] = 4
m[1][1] = 5
m[1][2] = 6
```

### Inizializzazione

Puoi inizializzarli al momento della dichiarazione usando parentesi graffe annidate.
```cpp
// Inizializzazione esplicita di un array 2x3
int matrice[2][3] = {
    { 1, 2, 3 },  // Riga 0
    { 4, 5, 6 }   // Riga 1
};

// Si può omettere la prima dimensione solo se si inizializza
int matrice2[][3] = {
    { 1, 2, 3 },
    { 4, 5, 6 }
}; // Il compilatore capisce che ci sono 2 righe

// ERRORE: non si possono omettere le dimensioni successive!
// int matrice_err[2][] = { ... }; // NON VALIDO
```

##### CIN e COUT nel blocco main:
```cpp
int main()  
{  
    int matrice[2][3];  
  
    //per il CIN  
    for (int i = 0; i < 2; i++){       //RIGHE
        for (int j = 0; j < 3; j++)    //COLONNE
            cin >> matrice[i][j];  
    }  
      
    //per il COUT  
	for (int i = 0; i < 2; i++){  
        for (int j = 0; j < 3; j++)  
            cout << matrice[i][j] << ' ';  
        cout << endl;  
    }  
    return 0;  
}
```

##### FUNZIONI PER STAMPA:
```cpp
#include <iostream>
using namespace std;

void stampa_matrice (int M[][3], int R)
{
    // R è ESSENZIALE per questo ciclo
    for (int r = 0; r < R; r++){
        // 3 è fisso (hardcoded)
        for (int c = 0; c < 3; c++)
            cout << M[r][c] << " ";
        cout << endl;
    }
}

void stampa_matrice2 (int* M, int R, int C)
{
    // R è ESSENZIALE per questo ciclo
    for (int r = 0; r < R; r++){
        // C è ESSENZIALE per questo calcolo
        for (int c = 0; c < C; c++)
            cout << *(M + r * C + c) << " ";
        cout << endl;
    }
}

int main()
{
    // L'inizializzazione { ... } è corretta,
    // ma la forma con doppie parentesi è più chiara:
    // int matrice[2][3] = { {1,3,4}, {5,6,3} };
    int matrice[2][3] = {1,3,4,5,6,3}; // matrice[0] = {1,3,4}, matrice[1] = {5,6,3}

    // --- Chiamata Corretta 1 ---
    // Funziona perché matrice è [][3] e R=2
    stampa_matrice(matrice, 2);
    
    cout << endl;

    // --- Chiamata Corretta 2 ---
    // Ora è corretto, passiamo C = 3
    stampa_matrice2(&matrice[0][0], 2, 3);


    cout<< endl << matrice[0][0] << endl; // Stampa il primo elemento (1)

    return 0;
}
```

---
### Inizializzare Matrici con Costanti

```cpp
int matrice[2][3];

for (int i = 0; i < 2; i++) { // Perché 2?
    for (int j = 0; j < 3; j++) { // Perché 3?
        cin >> matrice[i][j];
    }
}
stampa_matrice(matrice, 2); // Perché 2?
```
Qui, i numeri `2` e `3` sono chiamati "**numeri magici**". Compaiono ovunque nel codice (nella dichiarazione, nei cicli `for`, nelle chiamate a funzione).

**Cosa succede se vuoi cambiare la dimensione della matrice da 2x3 a 10x5?** Devi trovare e sostituire _manualmente_ogni `2` con `10` e ogni `3` con `5` in tutto il file, sperando di non dimenticarne nessuno. È un incubo e una fonte sicura di bug.

---
### 2. Il Metodo "con Costanti" (Raccomandato)

Questo è quasi certamente ciò che ha fatto il tuo professore (ed è quello che ho usato nel mio primo esempio). Si definiscono le dimensioni **una sola volta** all'inizio del programma, usando la parola chiave `const`.

`const` significa "costante", ovvero che il suo valore non potrà **mai** essere cambiato.

```cpp
#include <iostream>
using namespace std;

// 1. Definiamo le dimensioni UNA SOLA VOLTA
const int RIGHE = 2;
const int COLONNE = 3;

// Funzione che si basa sulla costante COLONNE
void stampa_matrice(int M[][COLONNE], int R) {
    for (int r = 0; r < R; r++) {
        for (int c = 0; c < COLONNE; c++) { // Usa la costante
            cout << M[r][c] << " ";
        }
        cout << endl;
    }
}

int main() {
    // 2. Usiamo le costanti per dichiarare
    int matrice[RIGHE][COLONNE];

    cout << "Inserisci i valori:" << endl;

    // 3. Usiamo le costanti nei cicli
    for (int i = 0; i < RIGHE; i++) {
        for (int j = 0; j < COLONNE; j++) {
            cin >> matrice[i][j];
        }
    }

    cout << "\nMatrice stampata:" << endl;
    
    // 4. Usiamo le costanti nella chiamata
    stampa_matrice(matrice, RIGHE);

    return 0;
}
```

### I Vantaggi di questo metodo:

1. **Manutenzione Facile:** Se ora vuoi una matrice 10x5, devi cambiare **solo 2 righe** all'inizio del file:

```cpp
const int RIGHE = 10;
const int COLONNE = 5;
```

- Tutto il resto del programma (i cicli `for`, le chiamate alle funzioni) si adatterà automaticamente. Non devi toccare nient'altro.
    
- **Leggibilità:** Il codice è molto più chiaro. `for (int i = 0; i < RIGHE; i++)` è più facile da capire di `for (int i = 0; i < 2; i++)`.
    
- **Sicurezza:** Le funzioni (come `stampa_matrice`) dipendono dalla costante `COLONNE`. Questo garantisce che la funzione e il `main` siano sempre "sincronizzati" sulla dimensione delle colonne. Non puoi commettere l'errore che hai fatto prima (passare `C=4` a una matrice che ne ha `3`).

--- 
---

5 Novembre
#### **gestione manuale della memoria dinamica (l'heap)** - tramite i puntatori.

In C++, "vettori in memoria dinamica" si riferisce a array la cui dimensione non è nota al momento della compilazione, ma viene decisa durante l'esecuzione del programma. Questi dati vengono allocati nello **heap** (o _free store_), una regione di memoria gestita manualmente, a differenza dello **stack** che gestisce automaticamente le variabili locali.

Ci sono due modi principali per gestire array dinamici in C++: usando i puntatori con `new[]` e `delete[]`

---
### Vettori in Memoria Dinamica

Normalmente, un array (vettore) in C++ ha una dimensione fissa decisa a _compile-time_, ad esempio `int mioVettore[10];`.

Con la **memoria dinamica**, puoi decidere la dimensione a _runtime_ (cioè mentre il programma è in esecuzione), usando l'operatore `new[]`.

- **Allocazione:** `int* h = new int[100];` (come nei tuoi appunti). Questo crea un array di 100 interi "nell'heap" (la memoria dinamica) e `h` è un puntatore che punta al primo elemento di questo array.
    
- **Deallocazione:** La memoria allocata con `new[]` **deve** essere liberata manualmente con `delete[]` quando non serve più, altrimenti si crea un _memory leak_. `delete[] h;`.
    
- **Variabile Singola:** I tuoi appunti mostrano anche `int* t = new int;`. Questo alloca spazio per un _singolo_ intero, non un array. Per liberarlo, si usa `delete` senza parentesi: `delete t;`.


#### **passare un puntatore per riferimento (`int* &pun`)**.

FUORI MAIN:
```cpp
void allocaMemoria(int* &pun, int dim) {  
    pun = new int[dim];         // Ora pun è una referenza a un puntatore (int* &)  
}  
  
void dellocaMemoria(int* &pun) {  
    delete[] pun;               // Dealloca l'array  
    pun = nullptr;              // Buona pratica: imposta il puntatore a null dopo la delete  
}
```
NEL MAIN: 
```cpp
int* vettDim = nullptr; // Inizializzato a "non punta a nulla"
int len;
cin >> len;
cout << vettDim << endl; // Stampa 0 (o 0x0), cioè l'indirizzo nullo

allocaMemoria([&]vettDim, len);
cout << vettDim << endl; // Stampa un indirizzo di memoria (es. 0x7ffc1234)

dellocaMemoria([&]vettDim);
cout << vettDim << endl; // Stampa di nuovo 0 (perché l'abbiamo rimesso a nullptr)
```

**Perché `int* &pun`?**

- Se la funzione fosse `void allocaMemoria(int* pun, int dim)`, `pun` sarebbe una _copia_ di `vettDim`.
    
- All'interno della funzione, `pun = new int[dim];` modificherebbe solo la _copia locale_.
    
- `vettDim` nel `main` rimarrebbe `nullptr`!


Usando `int* &pun` (puntatore **per riferimento**), `pun` diventa un _alias_ per `vettDim`. Qualsiasi modifica a `pun` all'interno della funzione (come assegnargli il nuovo indirizzo di memoria) modifica _direttamente_ `vettDim` nel `main`.

**Questa è la "tecnica del prof" per permettere a una funzione di cambiare _dove_ un puntatore punta.**


---

### Funzione `estendiVettore`

Non puoi "estendere" un array dinamico. Quello che devi fare è:

1. Creare un **nuovo** array più grande.
    
2. **Copiare** tutti gli elementi dal vecchio array al nuovo.
    
3. **Aggiungere** il nuovo elemento in fondo.
    
4. **Deallocare** il vecchio array.
    
5. Far puntare il puntatore originale al **nuovo** array.



Anche qui si usa `int* &pun` proprio per il punto 5: deve modificare `vettDim` nel `main`.
```cpp
void estendiVettoreDiUnaComponente(int* &pun, int dim, int a) {
    // 1. Alloca il nuovo array (dimensione vecchia + 1)
    int* nuovo = new int[dim + 1];

    // 2. Copia i vecchi elementi
    for (int i = 0; i < dim; i++) {
        nuovo[i] = pun[i];
    }
    // 3. Aggiungi il nuovo elemento in fondo
    nuovo[dim] = a;
    // 4. Dealloca il vecchio array
    delete[] pun;
    // 5. Fai puntare il puntatore originale al nuovo array
    //    (grazie a int* &, questo modifica vettDim nel main)
    pun = nuovo;
}
```
NEL MAIN: dopo averla chiamata, devi ricordarti di incrementare la variabile `len`, proprio come hai scritto:
```cpp
estendiVettoreDiUnaComponente(vettDim, len, 56);
++len; // Corretto! La dimensione logica ora è aumentata di 1
```

---

### Puntatori a Puntatori (`int**`)
Questo concetto è la base per le matrici dinamiche. È una catena:

- `int a`: una variabile che contiene un valore (es. 7).
    
- `int* p`: un puntatore. Contiene l'**indirizzo di una variabile `int`**.
    
- `int** q`: un puntatore a puntatore. Contiene l'**indirizzo di una variabile `int*`**.

```cpp
int a = 3;
int* p = &a; // p contiene l'indirizzo di a

(*p) = 5;    // (*p) significa "il valore a cui p punta" (cioè 'a'). Ora a = 5
cout << a; // Stampa 5

cout << p;  // Stampa l'indirizzo di 'a' (es. 0x7ffcABC0)
cout << &p; // Stampa l'indirizzo *di p stesso* (es. 0x7ffcABC8)

// --- Il Doppio Puntatore ---
int ** q = &p; // q contiene l'indirizzo di p

cout << q << endl;    // Stampa l'indirizzo di p (0x7ffcABC8)
cout << &q << endl;   // Stampa l'indirizzo di q (es. 0x7ffcABD0)

cout << (*q) << endl; // *q significa "il valore a cui q punta", che è 'p'.
                      // Stampa il valore di p (l'indirizzo di 'a', 0x7ffcABC0)

cout << *(*(q)) << endl; // *(*q) è come dire *(p), che è 'a'. Stampa 5

**q = 7;      // È come dire (*(*q)) = 7, cioè (*p) = 7, cioè a = 7
cout << a;    // Stampa 7
```


---

### Matrici Dinamiche e il "Direttorio

Qui è dove uniamo tutti i concetti. Come crei una matrice `mat[righe][colonne]` se non conosci `righe` e `colonne` prima?

**Non puoi** allocare un blocco 2D in un colpo solo. Devi creare un **"Vettore di Vettori"**.

L'idea è che `matrice` sarà un **doppio puntatore (`int**`)**. Questo puntatore (`matrice`) punterà al primo elemento di un **array di puntatori (`int*`)**. Questo array è il **"Direttorio"**.

Ogni puntatore nel "Direttorio" punterà al primo elemento di un **array di interi (`int`)**, che rappresenta una _riga_ della matrice.

**Visualizzazione (matrice 3x4):**

```
matrice (int**)
    |
    V
Direttorio [ptr_0, ptr_1, ptr_2]  (un array di int*)
              |      |      |
              |      |      +-> Riga 2 [_, _, _, _] (un array di int)
              |      +---------> Riga 1 [_, _, _, _] (un array di int)
              +----------------> Riga 0 [_, _, _, _] (un array di int)
```

**Esempio di codice (Allocazione):**
```cpp
int righe, colonne;
cout << "Inserisci righe e colonne: ";
cin >> righe >> colonne;

// 1. Dichiara il doppio puntatore
int** matrice = nullptr;

// 2. Alloca il "Direttorio" (un array di puntatori int*)
matrice = new int*[righe];

// 3. Alloca ogni singola riga (array di int)
for (int i = 0; i < righe; i++) {
    matrice[i] = new int[colonne];
}

// Ora puoi usarla come una matrice normale:
matrice[1][2] = 10;
```
---
**Deallocazione (fondamentale!):** Devi fare il processo al contrario. Prima deallochi tutte le righe, _poi_ deallochi il direttorio.
```cpp
// 1. Dealloca ogni singola riga
for (int i = 0; i < righe; i++) {
    delete[] matrice[i];
}

// 2. Dealloca il "Direttorio"
delete[] matrice;

// 3. Buona pratica
matrice = nullptr;
```



---

### Matrici Dinamiche (Vettore di Vettori)

Usiamo i doppi puntatori (`int**`) per creare matrici 2D. L'idea è creare un **"Vettore di Vettori"**.

`int** mat` -> punta a un array di `int*` (il **Direttorio**) Ciascun `int*` nel direttorio -> punta a un array di `int` (la **Riga**)

##### A. Matrice Rettangolare (`R` x `C`)
```cpp
/**
 * Alloca una matrice R x C sull'heap.
 */
void allocaMatriceSulloHeap(int** &mat, int R, int C) {
    // 1. Alloca il "Direttorio" (un array di R puntatori a intero)
    mat = new int*[R];
    
    // 2. Alloca ogni singola Riga (un array di C interi)
    for (int r = 0; r < R; r++) {
        mat[r] = new int[C];
    }
}

/**
 * Dealloca una matrice R x C.
 * (Bisogna specificare R per sapere quanto è lungo il direttorio)
 */
void deallocaMatriceDalloHeap(int** &mat, int R) {
    // 1. Dealloca ogni singola Riga
    for (int r = 0; r < R; r++) {
        delete[] mat[r];
    }
    
    // 2. Dealloca il "Direttorio"
    delete[] mat;
    
    // 3. Buona pratica
    mat = nullptr;
}
```

##### B. Matrice Irregolare (Jagged Array)
```cpp
int nRighe = 3;
int** q = new int*[nRighe]; // 1. Alloca il direttorio

for (int r = 0; r < nRighe; r++) {
    // 2. Alloca la riga 'r' con 'r+1' colonne
    q[r] = new int[r+1]; 
    // r=0 -> new int[1]
    // r=1 -> new int[2]
    // r=2 -> new int[3]
}

// ... uso della matrice ...
// q[0][0] = ...
// q[1][0] = ..., q[1][1] = ...
// q[2][0] = ..., q[2][1] = ..., q[2][2] = ...

// DEALLOCAZIONE (IMPORTANTE!)
// La 'delete[] q' nell'immagine è SBAGLIATA e causa MEMORY LEAK.
// Si dealloca come una matrice normale:
for (int r = 0; r < nRighe; r++) {
    delete[] q[r]; // Libera ogni singola riga
}
delete[] q; // Libera il direttorio
q = nullptr;
```

___

### Puntatori a Oggetti e Struct

Quando si usa un puntatore a un oggetto (es. `struct record`), ci sono due modi per accedere ai suoi membri.

```cpp
// Assumiamo esista: struct record { string nome; int eta; };
record r1;
r1.eta = 20;

record* p = &r1; // p punta a r1 (un oggetto sullo stack)

// Sintassi 1: Dereferenziazione e operatore '.'
cout << (*p).eta; // Stampa 20

// Sintassi 2: Operatore Freccia '->' (scorciatoia preferita)
cout << p->eta; // Stampa 20
```

###### Array di Oggetti sull'Heap
```cpp
record* q = nullptr;
q = new record[5]; // Alloca un array di 5 oggetti 'record' sull'heap

// Per accedere al primo oggetto e ai suoi membri:
// q[0] È un oggetto 'record', non un puntatore!
// Quindi si usa l'operatore PUNTO ('.')
strcpy(q[0].nome, "Giovanni"); // (strcpy si usa per le C-string, char[])
q[0].eta = 66;

// Per accedere al secondo oggetto:
q[1].eta = 166;

// Deallocazione corretta per un array di oggetti
delete[] q;
```





---





# LE LISTE 

```cpp
const MAX_DIM = 31;
struct record {
		char nome [MAX_DIM];
		char cognome [MAX_DIM];
		int età;
};

int main (){
	record r1, r2;
	strcpy (r1.nome, _Source: "Mario");
	strcpy (r1.cognome, _Source: "Rossi");
	r1.età = 44;
	
	r2 = r1;
	
	cout << r2.nome; //Mario
	
	record* p = & r1;
	cout << (*p).età;
	
	record* q = nullptr;
	q = new record;
	strcpy (p -> nome, _Source: "Giovanni");
	q -> età = 86;
	delete q;
return 0;
}
```


1. **`struct record`**: Definisce un modello `record` (nome, cognome, età).
    
2. **`record r1, r2;`**: Crea due oggetti `record` separati sullo **stack**.
    
3. **`strcpy(r1...);`**: Riempie `r1` con "Mario Rossi", 44.
    
4. **`r2 = r1;`**: **Copia** i valori da `r1` a `r2`. Ora `r2` contiene "Mario Rossi", 44, ma è un oggetto indipendente.
    
5. **`record* p = &r1;`**: Crea un puntatore `p` che punta all'indirizzo di `r1`.
    
6. **`record* q = new record;`**: Crea un _nuovo_ oggetto `record` (anonimo) sulla **heap** e fa puntare `q` ad esso.
    
7. **`strcpy(p->nome, ...);`**: Usa il puntatore `p` per **modificare `r1`**. `r1.nome` diventa "Giovanni". `r2` non cambia.
    
8. **`q->età = 86;`**: Modifica l'oggetto sulla **heap** (quello puntato da `q`).
    
9. **`delete q;`**: Libera la memoria allocata sulla **heap** (quella di `q`). Non serve `delete p` perché `r1` è sullo stack e si pulisce da solo.

---

--- 

6 nov
## PILA

In C++, una "pila" (in inglese **stack**) è una struttura dati fondamentale che funziona secondo il principio **LIFO** (Last In, First Out), ovvero "L'ultimo ad entrare è il primo ad uscire".

Puoi pensarla esattamente come una pila di piatti:

- Puoi **aggiungere** un piatto solo in cima (operazione `push`).
    
- Puoi **rimuovere** solo il piatto che si trova in cima (operazione `pop`).
    
- Puoi **guardare** solo il piatto che si trova in cima (operazione `top`).
    

Non puoi accedere o rimuovere un piatto nel mezzo della pila senza prima aver rimosso tutti quelli sopra di esso.

---
## 1. L'Idea di Base: la Struttura (`struct pila`)

L'idea è semplice: **rappresentiamo una pila usando un array e un indice**.
```cpp
const int DIM = 5; // Dimensione fissa del nostro array

struct pila {
    int top;          // L'indice della "cima"
    T stack[DIM];     // L'array che contiene i dati
};
```

- **`T stack[DIM]`**: È l'array fisico (il "contenitore", la pila di piatti) che contiene gli elementi. Ha una dimensione fissa (`DIM = 5`).
    
- **`int top`**: Questa è la variabile più importante. È un indice che ci dice **dove si trova l'ultimo elemento inserito** (la cima).

---

### La Regola Fondamentale degli Indici

Per far funzionare tutto, si usa una convenzione (mostrata nelle tue immagini):

- **Pila Vuota**: `top == -1`
    
    - Perché -1? Perché l'array inizia all'indice `0`. Se `top` è `-1`, significa che non c'è nessun elemento valido nell'array.
        
- **Pila Piena**: `top == DIM - 1`
    
    - Perché `DIM - 1`? Se la dimensione è 5 (`DIM = 5`), gli indici validi dell'array vanno da `0` a `4`. Quindi, quando `top`arriva a `4` (cioè `5 - 1`), la pila è piena.

---
## 2. Le Operazioni (le Funzioni)

Adesso vediamo come le funzioni usano la `struct` e la variabile `top` per simulare il comportamento LIFO.

### `void inip (pila& pp)` (Inizializzazione)
```cpp
void inip(pila& pp) {
    pp.top = -1;
}
```

- **Cosa fa**: Semplicemente imposta `top` a `-1`.
    
- **Perché**: Questa è la _prima_ funzione da chiamare. Dice alla nostra struttura "Ehi, sei vuota" e la prepara a ricevere il primo elemento.

---
### `bool empty (const pila& pp)` (Controllo Vuota)

```cpp
bool empty(const pila& pp) {
    if (pp.top == -1) return true;
    return false;
}
```

- **Cosa fa**: Controlla se `top` è uguale a `-1`.
    
- **Perché**: Se `top` è `-1`, per la nostra convenzione, la pila è vuota.
    

### `bool full (const pila& pp)` (Controllo Piena)

```cpp
bool full(const pila& pp) {
    if (pp.top == DIM - 1) return true;
    return false;
}
```

- **Cosa fa**: Controlla se `top` è arrivato all'ultimo indice disponibile (`DIM - 1`, che nel nostro caso è `4`).
    
- **Perché**: Dobbiamo saperlo per evitare di inserire (`push`) elementi in un array già pieno, il che causerebbe un errore (overflow).

---

### `bool push (pila& pp, T s)` (Inserimento)

Questa è l'operazione LIFO fondamentale.
```cpp
bool push(pila& pp, T s) {
    if (full(pp)) return false; // 1. Controlla se c'è spazio
    pp.stack[++(pp.top)] = s;   // 2. Il cuore dell'operazione
    return true;
}
```

Analizziamo la riga `pp.stack[++(pp.top)] = s;`:

1. **`++(pp.top)`**: Questa è un'operazione di **pre-incremento**. Significa "prima aumenta il valore di `top` di 1, e _poi_ usa il nuovo valore".
    
    - Se la pila era vuota: `top` era `-1`. Diventa `0`.
        
    - Se la pila aveva un elemento: `top` era `0`. Diventa `1`.
        
2. **`pp.stack[... ] = s`**: Dopo aver incrementato `top`, usa quel nuovo valore come indice per inserire l'elemento `s` nell'array.
    
    - Se la pila era vuota (`top = -1`), `top` diventa `0` e `s` viene messo in `pp.stack[0]`.
        
    - Se la cima era `stack[0]` (`top = 0`), `top` diventa `1` e `s` viene messo in `pp.stack[1]`.
        

**In pratica: "Prima sposta il puntatore della cima in su, poi metti il piatto in quella nuova posizione."**

### `bool pop (pila& pp, T& s)` (Estrazione)

Questa è la seconda operazione LIFO fondamentale.

```cpp
bool pop(pila& pp, T& s) {
    if (empty(pp)) return false; // 1. Controlla se c'è qualcosa da togliere
    s = pp.stack[(pp.top)--];    // 2. Il cuore dell'operazione
    return true;
}
```

Analizziamo la riga `s = pp.stack[(pp.top)--];`:

1. **`(pp.top)--`**: Questa è un'operazione di **post-decremento**. Significa "prima usa il valore _attuale_ di `top`, e _poi_ (dopo che l'intera riga è finita) diminuiscilo di 1".
    
2. **`s = pp.stack[...]`**: Prende l'elemento che si trova alla cima _attuale_ (es. `pp.stack[4]`) e lo copia nella variabile `s` (nota la `T& s`nei parametri: `s` è passato _per riferimento_ proprio per poter "restituire" il valore estratto).
    
3. **Dopo la copia**: L'indice `top` viene decrementato.
    
    - Se la cima era `stack[4]` (`top = 4`), il valore `stack[4]` viene copiato in `s`, e _poi_ `top` diventa `3`.
        

**In pratica: "Prendi il piatto che è in cima, e poi sposta il puntatore della cima sul piatto che stava sotto."**

> **Nota importante**: L'elemento non viene _veramente cancellato_ dall'array. Il valore rimane lì, ma siccome abbiamo spostato `top` indietro, quell'elemento è ora "invisibile" e verrà sovrascritto dal prossimo `push`.

---
### `void stampa (const pila& pp)` (Stampa)

```cpp
void stampa(const pila& pp) {
    cout << "Elementi contenuti nella pila: " << endl;
    for (int i = pp.top; i >= 0; i--) { // Parte dalla cima
        cout << '[' << i << "] " << pp.stack[i] << endl;
    }
}
```
- **Cosa fa**: Esegue un ciclo `for` che parte dall'indice `pp.top` (la cima) e scende fino all'indice `0` (il fondo), stampando ogni elemento.
    
- **Perché**: Mostra correttamente lo stato della pila, dall'ultimo inserito (cima) al primo inserito (fondo).

--- 

PROGRAMMA di esempio: 
```cpp
typedef int T;        //Definiamo il tipo 'T' come un intero

const int DIM = 5;    //Dim max della pila

struct pila {
    int top;          // Indice dell'elemento in cima
    T stack[DIM];     // L'array che contiene i dati
};

// --- Funzioni di gestione (dall'Immagine 1 & 2) ---

void inip(pila& pp) {             //INIZIALIZZO PILA
    pp.top = -1;
}


bool empty(const pila& pp) {      //Controlla se la pila è vuota.
    if (pp.top == -1) 
        return true;
    else                     // Si poteva anche scrivere: return (pp.top == -1);
        return false;
    
}


bool full(const pila& pp) {      //Controlla se la pila è piena. 
    if (pp.top == DIM - 1)
        return true;
    else               // Si poteva anche scrivere: return (pp.top == DIM - 1);
        return false;
}


 //Inserisce un elemento in cima alla pila.
bool push(pila& pp, T s) {
    if (full(pp)) {
        return false; // Errore: pila piena
    }
    // Pre-incremento: prima aumenta 'top', poi lo usa come indice
    pp.stack[++(pp.top)] = s;
    return true;
}


 //Estrae un elemento dalla cima della pila.
bool pop(pila& pp, T& s) {
    if (empty(pp)) {
        return false; // Errore: pila vuota
    }
    // Post-decremento: prima usa 'top' come indice, poi lo diminuisce
    s = pp.stack[(pp.top)--];
    return true;
}

/**
 * @brief Stampa il contenuto della pila, dalla cima al fondo.
 * @param pp Riferimento (costante) alla pila.
 */
void stampa(const pila& pp) {
    cout << "Elementi contenuti nella pila: " << endl;
    // Il ciclo parte dalla cima (top) e scende fino a 0
    for (int i = pp.top; i >= 0; i--) {
        cout << '[' << i << "] " << pp.stack[i] << endl;
    }
}



int main() {
    pila st;    // 1. Dichiara una pila
    inip(st);   // 2. La inizializza (ora top = -1)
    T num;      // 3. Variabile d'appoggio per il pop

    
    if (empty(st))                        // 4. Controlla se è vuota
        cout << "Pila vuota" << endl;

	// 5. Primo ciclo: riempie la pila
    // Inserisce 4, 3, 2, 1, 0
    for (int i = 0; i < DIM; i++) {
        if (push(st, DIM - 1 - i))
            cout << "Inserito " << (DIM - 1 - i) 
                 << ". Valore di top: " << st.top << endl;
        else
            cerr << "Inserimento di " << i << " fallito" << endl;
    }


    if (full(st))                          // 6. Controlla se è piena
        cout << "Pila piena" << endl;

    stampa(st);       // 7. Stampa la pila (stamperà 0, 1, 2, 3, 4)

    // 8. Secondo ciclo: rimuove 3 elementi (DIM - 2 = 3)
    for (int i = 0; i < DIM - 2; i++) {
        if (pop(st, num))
            cout << "Estratto " << num 
                 << ". Valore di top: " << st.top << endl;
        else
            cerr << "Estrazione fallita" << endl;
    }
    // La pila ora contiene [4, 3] (con 3 in cima, top = 1)

    // 9. Terzo ciclo: tenta di inserire 5 nuovi elementi
    // Inserirà 0, 1, 2. Fallirà con 3 e 4.
    for (int i = 0; i < DIM; i++) {
        if (push(st, i))
            cout << "Inserito " << i 
                 << ". Valore di top: " << st.top << endl;
        else
            cerr << "Inserimento di " << i << " fallito" << endl;
    }
    // La pila ora contiene [4, 3, 0, 1, 2] (con 2 in cima, top = 4)

    // 10. Stampa la pila (stamperà 2, 1, 0, 3, 4)
    stampa(st);

    // 11. Quarto ciclo: rimuove 2 elementi
    for (int i = 0; i < 2; i++) {
        if (pop(st, num))
            cout << "Estratto " << num 
                 << ". Valore di top: " << st.top << endl;
        else
            cerr << "Estrazione fallita" << endl;
    }
    // La pila ora contiene [4, 3, 0] (con 0 in cima, top = 2)

    stampa(st);   // 12. Stampa finale
    
    return 0; // Termina il programma
}
```

IN USCITA: 
```
Pila vuota
Inserito 5. Valore di top: 0
Inserito 4. Valore di top: 1
Inserito 3. Valore di top: 2
Inserito 2. Valore di top: 3
Inserito 1. Valore di top: 4
Pila piena
Elementi contenuti nella pila:
[4] 1
[3] 2
[2] 3
[1] 4
[0] 5
Estratto 1. Valore di top: 3
Estratto 2. Valore di top: 2
Estratto 3. Valore di top: 1
Inserito 0. Valore di top: 2
Inserito 1. Valore di top: 3
Inserito 2. Valore di top: 4
Inserimento di 3 fallito
Inserimento di 4 fallito
Elementi contenuti nella pila:
[4] 2
[3] 1
[2] 0
[1] 4
[0] 5
Estratto 2. Valore di top: 3
Estratto 1. Valore di top: 2
Elementi contenuti nella pila:
[2] 0
[1] 4
[0] 5
```

---
---

## CODA
(opposto concettuale di PILA)
## 1. Il Concetto: Coda (FIFO)

A differenza della Pila (LIFO), la Coda funziona secondo il principio **FIFO** (First In, First Out), ovvero "Il primo ad entrare è il primo ad uscire".

Pensa a una fila (una coda) in un negozio:

- La prima persona che si mette in fila (`front`) è la prima ad essere servita e ad uscire.
    
- Le nuove persone si aggiungono sempre in fondo (`back`).

---
### L'Implementazione: Array Circolare

Per non dover "spostare" tutti gli elementi nell'array ogni volta che ne rimuovi uno (operazione lenta), si usa un **array circolare**.

Non è una struttura speciale, è solo un modo di _gestire gli indici_ di un array normale. Si usano due "puntatori" (che in realtà sono solo indici):

- **`front`**: L'indice della "testa" della coda. Da qui avvengono le **estrazioni** (`esqueque`).
    
- **`back`**: L'indice della "coda" della fila. Qui avvengono gli **inserimenti** (`insqueue`).


La magia avviene con l'operatore **modulo (`%`)**, che fa "girare" l'indice, permettendo all'array di "ripartire da capo" quando arriva alla fine.

---

## 2. La Struttura e le Funzioni Base

La `struct` è molto simile a quella della pila, ma con due indici invece di uno.
```cpp
const int DIM = 5;

struct coda {
    int front, back;  // Due indici
    T queue[DIM];     // L'array
};

void inic(coda& cc) {
    // All'inizio, testa e coda puntano entrambi a 0
    cc.front = cc.back = 0;
}
```

---
### Le Condizioni: Vuota e Piena (Il trucco)

Qui sta la parte più importante:

- **`bool empty(const coda& cc)`**:
    
    - `if (cc.front == cc.back) return true;`
        
    - Se `front` e `back` puntano allo stesso posto, la coda è vuota.
        
- **`bool full(const coda& cc)`**:
    
    - `if (cc.front == (cc.back + 1) % DIM) return true;`
        
    - **ATTENZIONE**: Per distinguere la condizione "vuota" (dove `front == back`) dalla condizione "piena", si decide che la coda è piena quando `back` è _subito dietro_ `front`.
        
    - Questo significa che **si lascia sempre uno spazio vuoto**. Una coda con `DIM = 5` può contenere al massimo **`DIM - 1` = 4** elementi.

---

## 3. Le Operazioni Principali (FIFO)

### `bool insqueue (coda& cc, T s)` (Inserimento)

Aggiunge un elemento _in fondo_ alla coda (`back`).
```cpp
bool insqueue(coda& cc, T s) {
    if (full(cc)) return false; // 1. Controlla se c'è spazio
    cc.queue[cc.back] = s;      // 2. Inserisce l'elemento dove punta 'back'
    cc.back = (cc.back + 1) % DIM; // 3. Sposta 'back' in avanti (girando se serve)
    return true;
}
```

- `cc.back = (cc.back + 1) % DIM;` è il cuore. Se `back` è 4 e `DIM` è 5, `(4 + 1) % 5` diventa `0`. `back` "torna all'inizio".
---
### `bool esqueque(coda& cc, T& s)` (Estrazione)

Rimuove un elemento _dalla testa_ della coda (`front`).
```cpp
bool esqueque(coda& cc, T& s) {
    if (empty(cc)) return false; // 1. Controlla se c'è qualcosa
    s = cc.queue[cc.front];      // 2. Prende l'elemento dove punta 'front'
    cc.front = (cc.front + 1) % DIM; // 3. Sposta 'front' in avanti (girando se serve)
    return true;
}
```
PROGRAMMA slide B-255.