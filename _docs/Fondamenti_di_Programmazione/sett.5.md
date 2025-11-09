---
layout: default
title: Settimana 5 - Puntatori
parent: Fondamenti di Programmazione
nav_order: 5
---

# PUNTATORI

### Cosa sono i Puntatori?

Immagina la memoria del tuo computer (la RAM) come una lunghissima fila di scatole numerate. Ogni scatola ha un **indirizzo** unico (il suo numero) e può contenere un **valore**.

- Una **variabile normale** (es. `int a = 10;`) è una scatola a cui hai dato un nome (`a`). Il suo valore è `10`. Il suo indirizzo è dove si trova in memoria (es. `4080`).

- Una **variabile puntatore** è una scatola speciale il cui valore non è un numero normale, ma è l'**indirizzo di un'altra scatola**.


> **Analogia:** Un puntatore è come il numero di pagina nell'indice di un libro. Non è l'informazione stessa, ma ti dice _esattamente dove andare_ per trovarla.

### 2. Dichiarazione e Operatori Fondamentali (`&` e `*`)

Per lavorare con i puntatori, usiamo due operatori fondamentali:

1. **`&` (Operatore di indirizzo):** Si usa su una variabile normale per chiederle: "Qual è il tuo indirizzo?".

2. **`*` (Operatore di indirezione o dereferenziazione):** Si usa su un puntatore per dire: "Vai all'indirizzo che contieni e dammi il valore che c'è lì".


Esempio che combina tutto (come slide B-179):
```cpp
	int main() {
    int numero = 25;

    int *p_numero;       //Dichiariamo un puntatore

    // Assegnazione: l'operatore & (indirizzo di)
    // "Assegna a p_numero l'indirizzo (&) della variabile 'numero'"
    p_numero = &numero;

    // 'numero' vale 25
    // 'p_numero' vale l'indirizzo di 'numero' (es. 0x7ffee...)

    cout << "Valore di 'numero': " << numero << endl;       //25
    cout << "Indirizzo di 'numero': " << &numero << endl;   // Stampa l'indirizzo
    cout << "Valore di 'p_numero': " << p_numero << endl;   // Stampa lo stesso indirizzo!
    
    // 4. Dereferenziazione: l'operatore * (valore puntato)
    // "Mostrami il valore (*) che si trova all'indirizzo contenuto in 'p_numero'"
    cout << "Valore puntato da 'p_numero': " << *p_numero << endl; // Stampa 25

    // 5. Modificare il valore TRAMITE il puntatore
    // "Vai all'indirizzo in 'p_numero' e metti 100 in quella scatola"
    *p_numero = 100; 
    
    // La scatola 'numero' è stata modificata!
    cout << "Nuovo valore di 'numero': " << numero << endl; // 100

    return 0;
}
```

### 3. Pericolo: Puntatori Non Inizializzati e `nullptr`

`nullptr` è una **parola chiave** introdotta nello standard C++11. Rappresenta un **"puntatore nullo"** (un puntatore che non punta a niente) in modo sicuro e non ambiguo.

È il modo moderno e preferito per inizializzare un puntatore che non ha ancora un indirizzo valido o per segnare un puntatore che non punta più a un oggetto valido.


Come nella slide B-179 (alla fine, `int *p3;`), un puntatore dichiarato ma non inizializzato è **pericolosissimo**.

`int *p3;`

Cosa contiene `p3`? Spazzatura. Un indirizzo casuale. Se provi a usarlo (`*p3 = 10;`), stai scrivendo `10` in una zona di memoria a caso, quasi certamente causando un crash (Segmentation Fault).

Per evitare questo, si usa `nullptr` (la "parola chiave" menzionata nel tuo programma). È lo standard C++ moderno (dal 2011) per dire: "Questo puntatore non punta intenzionalmente da nessuna parte".
```cpp
int *p_pericolo;             // *p_pericolo = 5; // CRASH!

int *p_sicuro = nullptr;     // Pratica CORRETTA

// poi nel codice possiamo assegnargli un indirizzo valido
int valore = 10;
p_sicuro = &valore;
```

### 4. Puntatori come Argomenti di Funzioni (Effetto Collaterale)

Qui sta la vera potenza dei puntatori.

- **Effetto Principale:** Il valore che una funzione `return` (restituisce).
    
- **Effetto Collaterale:** Qualsiasi modifica che la funzione fa all'esterno di sé stessa (es. stampare a video, modificare una variabile globale, o **modificare una variabile del chiamante**).


Di default, C++ passa le variabili "per copia". Se passi `int a` a una funzione, lei lavora su una copia e l'originale non cambia.

Ma se passi un **puntatore** (l'indirizzo), la funzione può usare l'operatore `*` per modificare la variabile _originale_.

Questo è l'esempio `incrementa(int* p)` del tuo programma: ^incrementaintp
```cpp
// Questa funzione NON ha un effetto principale (è void)
// ma ha un potente EFFETTO COLLATERALE.
// Riceve l'indirizzo di un intero.
void incrementa(int *p) {
    // "Vai all'indirizzo p, prendi il valore (*p),
    // aggiungi 1, e rimettilo nello stesso posto (*p = ...)"
    *p = *p + 1;
}

int main() {
    int a = 10;
    
    cout << "Valore di 'a' prima: " << a << endl; // Stampa 10
    
    // Non passo 'a' (il valore 10)
    // Passo '&a' (l'indirizzo di 'a')
    incrementa(&a); 
    
    cout << "Valore di 'a' dopo: " << a << endl;  // Stampa 11
    
    return 0;
}
```


### 5. Funzioni che Ritornano Puntatori

Una funzione può anche _restituire_ un puntatore (un indirizzo). Questo è utile per "restituire" un oggetto che esiste già da qualche parte (come un elemento di un array).

> **Attenzione:** Non restituire MAI l'indirizzo di una variabile locale alla funzione. Quella variabile viene distrutta quando la funzione termina e il puntatore restituito punterebbe a spazzatura (si chiama "dangling pointer").

```cpp
// Esempio SBAGLIATO
int* creaIntero() {
    int x = 10;
    return &x; // ERRORE! 'x' muore alla fine della funzione
}

// Esempio CORRETTO (trova un elemento in un array esistente)
int* trovaValore(int* array, int dimensione, int valore) {
    for (int i = 0; i < dimensione; i++) {
        if (array[i] == valore) {
            return &array[i]; // Giusto! L'array esiste ancora nel main
        }
    }
    return nullptr; // Non trovato
}

int main() {
    int mioArray[] = {10, 20, 30, 40};

    int *puntatoreAl30 = trovaValore(mioArray, 4, 30);

    if (puntatoreAl30 != nullptr) {
        cout << "Trovato valore: " << *puntatoreAl30 << endl; // Stampa 30
        *puntatoreAl30 = 35; // Modifico l'array originale
    }

    cout << "Nuovo valore nell'array: " << mioArray[2] << endl; // Stampa 35

    return 0;
}
```

### 6. Puntatore a Puntatore (Doppio Puntatore)

Questo concetto è più semplice di quanto sembri.

- `int *p1` è un puntatore a un intero (contiene un indirizzo di `int`).
    
- `int **p2` è un puntatore a un puntatore a un intero (contiene un indirizzo di `int*`).
    

È un indirizzo che punta a un altro indirizzo.

```cpp
int main() {
    int valore = 5;
    
    // Livello 1: Puntatore a valore
    int *p1 = &valore;
    
    // Livello 2: Puntatore a puntatore
    int **p2 = &p1;

    cout << "Valore originale: " << valore << endl; // 5

    // Per raggiungere il valore, devi dereferenziare due volte:
    // *p2  -> mi dà 'p1' (l'indirizzo)
    // **p2 -> mi dà '*p1' (il valore 5)
    cout << "Valore tramite **p2: " << **p2 << endl; // 5

    // Posso usarlo per modificare 'valore'
    **p2 = 99;
    
    cout << "Valore modificato: " << valore << endl; // 99
    
    return 0;
}
```


---

### OPERATORE DI INDIREZIONE (o DEREFERENZIAZIONE)

L'operatore di indirezione (o dereferenziazione) è il simbolo **`*`** (asterisco) usato su una variabile puntatore.

La sua funzione è l'opposto dell'operatore di indirizzo (`&`):

- **`&` (Indirizzo):** Chiede "Qual è il tuo indirizzo?". Si applica a una variabile normale.
    
- **`*` (Indirezione):** Dice "Vai all'indirizzo che contieni e dammi il valore che c'è lì". Si applica a un puntatore.


---

### Come funziona

Immagina un puntatore come un biglietto con sopra scritto l'indirizzo di una casa.
```cpp
int casa = 100;         // Una casa (variabile) che contiene il valore 100
int* biglietto = &casa; // Un biglietto (puntatore) che contiene l'indirizzo di 'casa'
```
Se vuoi sapere cosa c'è _sul biglietto_ (l'indirizzo), leggi `biglietto`:
```cpp
cout << biglietto; // Stampa l'indirizzo di memoria (es: 0x7ffee...)
```
Se vuoi sapere cosa c'è _dentro la casa_ a cui punta il biglietto, usi l'operatore di indirezione **`*`**:
```cpp
cout << *biglietto; // Stampa 100
```
### Usarlo per Scrivere (Modificare)

L'operatore `*` non serve solo a _leggere_ il valore, ma anche a _modificarlo_. Quando usi `*puntatore` sul lato sinistro di un'assegnazione, stai modificando la variabile originale.

Questo è ciò che la tua slide (B-179) intende con "restituisce un l-value": significa che `*p1` si comporta esattamente come la variabile `i` originale e può quindi starci a sinistra di un `=` per ricevere un nuovo valore.

```cpp
int main() {
    int i = 1;
    int* p1 = &i; // p1 punta a i

    cout << "Valore di i: " << i << endl;   // Stampa 1
    cout << "Valore *p1: " << *p1 << endl; // Stampa 1

    // Ora modifichiamo 'i' USANDO il puntatore
    // "Vai all'indirizzo p1 e metti il valore 10 in quella scatola"
    *p1 = 10; 

    cout << "Valore di i dopo la modifica: " << i << endl; // Stampa 10
    
    return 0;
}
```

### Attenzione

L'errore più comune (e pericoloso) è usare l'operatore `*` su un puntatore che non punta a niente di valido (cioè un puntatore non inizializzato o un `nullptr`).
```cpp
int* p; // Non inizializzato, contiene un indirizzo casuale
*p = 5; // ERRORE GRAVE (Crash!) Stai scrivendo 5 in un posto a caso.

int* p_nullo = nullptr;
*p_nullo = 5; // ERRORE GRAVE (Crash!) Stai provando a scrivere all'indirizzo "nullo".
```
Usa `*` solo quando sei sicuro che il puntatore contenga un indirizzo di memoria valido.


---


### FUNZIONI CHE RITORNANO UN VALORE

Una **funzione che ritorna un valore** è un blocco di codice che, una volta completata la sua esecuzione, "restituisce" un singolo risultato al chiamante.

Questo risultato è il suo **Effetto Principale**.

---

### 1. L'Effetto Principale (Il `return`)

A differenza delle funzioni `void` (come `void incrementa(int* p)`), che non restituiscono nulla, la maggior parte delle funzioni viene scritta per calcolare un valore e "passarlo indietro".

Per farlo, si usano due cose:

1. **Tipo di Ritorno:** Nella dichiarazione della funzione, al posto di `void`, si specifica il tipo di dato che la funzione restituirà (es. `int`, `double`, `bool`, o anche un puntatore come `int*`).
    
2. **Parola chiave `return`:** All'interno della funzione, `return` è l'istruzione che termina la funzione e "spedisce" il valore indietro.


#### Esempio: Una funzione `somma`

Funzione che prende due interi e ne restituisce la somma.
```cpp
// Questa funzione "promette" di restituire un 'int'
int somma(int a, int b) {
    int risultato = a + b;
    
    // 'return' fa due cose:
    // 1. Termina immediatamente l'esecuzione della funzione
    // 2. Restituisce il valore 'risultato' al chiamante
    return risultato; 
}

int main() {
    // Chiamiamo la funzione e "catturiamo" il valore 
    // restituito nella variabile 'mioRisultato'.
    int mioRisultato = somma(5, 3);
    
    cout << "Il risultato (effetto principale) e': " << mioRisultato << endl; // Stampa 8

    return 0;
}
```


### 2. Effetto Principale vs. Effetto Collaterale

Questo è un concetto chiave del programma:

- **Effetto Principale:** È il valore che la funzione **restituisce** tramite `return`. È il suo scopo di calcolo primario.

- **Effetto Collaterale:** È _qualsiasi altra cosa_ che la funzione fa e che modifica lo stato del programma all'esterno della funzione stessa. Esempi:
    
    - Stampare a video (`cout`).
        
    - Leggere da tastiera (`cin`).
        
    - Modificare una variabile globale.
        
    - Modificare una variabile del chiamante tramite un **puntatore** o un **riferimento**.


### 3. Esempi a Confronto

Differenza usando l'idea di "incrementare un valore", come da tuo programma.

#### Esempio A: Funzione con Effetto Principale (usa `return`)

Questa funzione non modifica la variabile originale. Calcola un nuovo valore e lo restituisce.
```cpp
#include <iostream>
using namespace std;

// Funzione con Effetto PrincipALE
// Prende una copia di 'n', restituisce un 'int'
int incrementa_con_return(int n) {
    return n + 1; // Effetto Principale
}

int main() {
    int x = 10;
    
    cout << "x prima: " << x << endl; // 10
    
    // Per usare il risultato, devo assegnarlo di nuovo a 'x'
    x = incrementa_con_return(x); 
    
    cout << "x dopo: " << x << endl; // 11

    return 0;
}```cpp
```

#### Esempio B: Funzione con Effetto Collaterale (usa puntatori)

Questa è la funzione `incrementa(int* p)` del tuo programma. È `void` (nessun effetto principale), ma modifica l'originale tramite un puntatore.

```cpp
#include <iostream>
using namespace std;

// Funzione con Effetto COLLATERALE
// Non restituisce nulla (void), ma modifica l'originale
void incrementa_con_puntatore(int* p) {
    *p = *p + 1; // Effetto Collaterale: modifica la variabile del chiamante
}

int main() {
    int x = 10;
    
    cout << "x prima: " << x << endl; // 10
    
    // Passo l'indirizzo di 'x'
    incrementa_con_puntatore(&x); 
    
    cout << "x dopo: " << x << endl; // 11 (x è stata modificata)

    return 0;
}
```

Entrambi gli approcci hanno portato `x` da 10 a 11, ma in modi concettualmente molto diversi. La versione con `return`(Effetto Principale) è spesso più facile da leggere e da capire, perché il flusso dei dati è esplicito.


---


# Argomenti DEFAULT per funzioni

### 1. Come Funziona

Si specificano i valori di default nella **dichiarazione** (o prototipo) della funzione, usando l'operatore `=`.

La regola fondamentale è: **tutti i parametri con un valore di default devono essere messi alla fine della lista dei parametri.**
```cpp
// Regola: i parametri default vanno alla fine
//                      vvvvvvvvvvvvvvv
void miaFunzione(int a, int b, int c = 10, int d = 5);
```


- **OK:** `void fn(int a, int b = 5, int c = 10);`
    
- **ERRORE:** `void fn(int a = 5, int b);` (Errore! Quello default non è alla fine)
    

---

### 2. Esempio Pratico

Immaginiamo una funzione che calcola il prezzo totale di un prodotto, con una tassa opzionale. Se la tassa non viene specificata, applichiamo un'IVA standard del 22%.
```cpp
// Dichiarazione della funzione con argomento default
// 'prezzo' è obbligatorio
// 'tassa' è opzionale, se non fornito vale 0.22 (cioè 22%)
double calcolaPrezzoFinale(double prezzo, double tassa = 0.22) {
    double importoTassa = prezzo * tassa;
    return prezzo + importoTassa;
}

int main() {
    double prezzoProdotto = 100.0;

    // 1. Chiamata SENZA fornire l'argomento default
    // La funzione userà automaticamente tassa = 0.22
    double prezzoTassato = calcolaPrezzoFinale(prezzoProdotto);
    
    cout << "Prezzo con tassa standard (22%): " 
         << prezzoTassato << " EUR" << endl; // Stampa 122

    
    // 2. Chiamata FORNENDO l'argomento
    // Il valore 0.10 "sovrascrive" il default 0.22
    double prezzoSpeciale = calcolaPrezzoFinale(prezzoProdotto, 0.10);

    cout << "Prezzo con tassa speciale (10%): " 
         << prezzoSpeciale << " EUR" << endl; // Stampa 110

    return 0;
}
```

---

### OVERLOADING FUNZIONI

L'**overloading delle funzioni** (o _sovraccarico_) è una potente funzionalità del C++ che ti permette di definire **più funzioni con lo stesso nome**, a condizione che abbiano una **lista di parametri diversa**.

```cpp
void stampa(int valore) {            // Versione per gli interi
    cout << "Intero: " << valore << endl;
}

void stampa(double valore) {         // Versione per i double
    cout << "Double: " << valore << endl;
}

void stampa(string valore) {         // Versione per le stringhe
    cout << "Stringa: '" << valore << "'" << endl;
}
```



---



# ARRAY

n C++, il nome di un array è un puntatore al suo primo elemento.

### 1. I Vettori come Array Monodimensionali

In C++ (e C), il termine "vettore" è sinonimo di **array statico monodimensionale**.

- **Cos'è?** È un contenitore di dati **omogeneo** (tutti elementi dello stesso tipo, es. tutti `int`) e **contiguo** (gli elementi sono memorizzati uno dopo l'altro in memoria, senza spazi).
    
- **Come si dichiara?** Si usa la sintassi `tipo nome[dimensione];`.

```cpp
int main() {
    
    int voti[5];    // Dichiaro un array di 5 interi

    // Come accedo agli elementi? Con l'indice [].
    // Gli indici partono SEMPRE da 0.
    
    voti[0] = 30; // Primo elemento
    voti[1] = 28; // Secondo elemento
    voti[2] = 25;
    voti[3] = 27;
    voti[4] = 29; // Quinto (e ultimo) elemento

    // voti[5] = 20; // ERRORE GRAVE! (Buffer overflow)
    // Sto scrivendo in una memoria che non mi appartiene.

    cout << "Il secondo voto e': " << voti[1] << endl; // Stampa 28

    return 0;
}
```


### 2. Inizializzazione, Lettura e Stampa

Non puoi leggere o stampare un intero array in un colpo solo (es. `cout << voti;` non funziona come si pensa). Devi farlo elemento per elemento, usando un **ciclo `for`**.
```cpp
const int DIM_V = 5;    //BUONA PRATICA def la dimensione come costante

int main() {
   
    int voti[DIM_V] = {30, 28, 25, 27, 29};
    
    // Posso anche omettere la dimensione se inizializzo:
    // int voti[] = {30, 28, 25, 27, 29}; // Il compilatore capisce N=5

    // 2. Lettura di un vettore da tastiera
    // Dichiaro un altro array, stavolta vuoto
    int eta[DIM_V];
    cout << "Inserisci " << N << " eta':" << endl;
    
    for (int i = 0; i < N; i++) {
        cout << "Valore " << i << ": ";
        cin >> eta[i]; // Leggo un elemento alla volta
    }

    // 3. Stampa di un vettore a video
    cout << "Hai inserito queste eta': ";
    for (int i = 0; i < N; i++) {
        cout << eta[i] << " "; // Stampo un elemento alla volta
    }
    cout << endl;

    return 0;
}
```

### 3. Aritmetica dei Puntatori e Vettori

Ecco il punto chiave. Quando dichiari `int voti[5]`, il nome `voti` **non è l'array intero**. È un **puntatore costante** (`int*`) che punta all'indirizzo del primo elemento (`&voti[0]`).

```cpp
int voti[5] = {10, 20, 30, 40, 50};

// 'voti' è un puntatore, quindi posso assegnarlo a un altro puntatore
int* p = voti; // NOTA: non serve la & !
               // 'voti' È GIÀ un indirizzo.
               // È equivalente a: int* p = &voti[0];
```

Ora, cosa succede se "sommo" un numero a un puntatore? Questa è l'**Aritmetica dei Puntatori**.

Se `p` punta a `voti[0]`, `p + 1` non significa "indirizzo + 1 byte". Significa "spostati avanti di 1 _elemento_". Il compilatore sa che `p` è un `int*`, quindi "spostarsi di 1" significa avanzare di `sizeof(int)` byte (di solito 4).

- `p` punta a `voti[0]`
    
- `p + 1` punta a `voti[1]`
    
- `p + 2` punta a `voti[2]`
    
- ...e così via.


### 4. Operare su Vettori con un Puntatore (Senza `[]`)

Se `p` punta a `voti[0]`, come ottengo il valore `10`? Con l'operatore di dereferenziazione `*`:

- `*p` è `10` (è il valore di `voti[0]`)


E come ottengo il secondo valore, `20`?

- `p + 1` è l'indirizzo di `voti[1]`
    
- `*(p + 1)` è il valore `20` (è il valore di `voti[1]`)


Da questo deriva l'equivalenza fondamentale del C++:

**`voti[i]` è la stessa identica cosa di `*(voti + i)`**

L'operatore `[]` è solo una "scorciatoia" (zucchero sintattico) per l'aritmetica dei puntatori.

Vediamo come stampare un vettore usando solo i puntatori:

```cpp
int main() {
    const int N = 5;
    int voti[N] = {10, 20, 30, 40, 50};

    // 1. Modo classico con indici []
    cout << "Stampa con indici: ";
    for (int i = 0; i < N; i++) {
        cout << voti[i] << " ";
    }
    cout << endl;

    // 2. Modo con puntatore e aritmetica (senza [])
    cout << "Stampa con puntatore: ";
    int* p = voti; // p punta a voti[0]
    for (int i = 0; i < N; i++) {
        // All'iterazione i=0, stampa *(p + 0) -> *p -> 10
        // All'iterazione i=1, stampa *(p + 1) -> 20
        // All'iterazione i=2, stampa *(p + 2) -> 30
        cout << *(p + i) << " ";
    }
    cout << endl;

    return 0;
}
```

### 5. Funzioni che Operano su Vettori

Come si passa un array a una funzione? Non puoi passare "tutto l'array". Passi solo il **puntatore al suo primo elemento**(il nome dell'array, `voti`) e, siccome la funzione non sa quanto è grande l'array, devi passarle anche la **dimensione**.

Nella dichiarazione della funzione, scrivere `int v[]` o `int* v` è **perfettamente identico**.

```cpp
// --- Dichiarazioni delle funzioni ---

// Una funzione che stampa un vettore
// 'v' è un puntatore al primo elemento
void stampaVettore(int* v, int dim) {
    for (int i = 0; i < dim; i++) {
        cout << v[i] << " "; // Posso usare [] anche qui
    }
    cout << endl;
}

// Trova il valore massimo
int trovaMassimo(int* v, int dim) {
    int max = v[0]; // Assumo che il primo sia il max
    for (int i = 1; i < dim; i++) { // Parto dal secondo
        if (v[i] > max) {
            max = v[i]; // Trovato uno più grande!
        }
    }
    return max; // Effetto Principale (return)
}

// Trova la POSIZIONE (indice) del minimo
int posMinimo(int* v, int dim) {
    int min = v[0];
    int pos = 0; // Memorizzo l'indice
    for (int i = 1; i < dim; i++) {
        if (v[i] < min) {
            min = v[i];
            pos = i; // Aggiorno anche la posizione
        }
    }
    return pos;
}

// Funzione che scambia due elementi
// Questa ha un EFFETTO COLLATERALE: modifica l'array originale!
// Lo fa perché 'v' è un puntatore che punta all'array del main.
void scambia(int* v, int i, int j) {
    int temp = v[i]; // Variabile temporanea (bicchiere)
    v[i] = v[j];
    v[j] = temp;
}

// --- Funzione Principale ---
int main() {
    const int N = 6;
    int dati[N] = {40, 10, 50, 20, 30, 15};

    cout << "Vettore originale: ";
    stampaVettore(dati, N);

    cout << "Valore massimo: " << trovaMassimo(dati, N) << endl; // 50
    cout << "Posizione del minimo: " << posMinimo(dati, N) << endl; // 1 (perché 10 è in pos 1)

    cout << "Scambio l'elemento in pos 0 (40) con quello in pos 2 (50)..." << endl;
    scambia(dati, 0, 2);

    cout << "Vettore modificato: ";
    stampaVettore(dati, N); // Stampa: 50 10 40 20 30 15

    return 0;
}
```