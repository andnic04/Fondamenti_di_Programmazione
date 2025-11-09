---
layout: default
title: Settimana 6 - C-Stringhe
parent: Fondamenti di Programmazione
nav_order: 6
---

# 1. Le Stringhe C (c-stringhe)

Questo è il modo "vecchio" di gestire il testo in C/C++. È fondamentale capire che **non sono un tipo di dato magico**, ma una convenzione.

 **Concetto Chiave: Vettore di `char` + Terminatore `\0`** 
 
 Una cstringa è semplicemente un **vettore (array) di caratteri** che ha una regola speciale: l'ultimo carattere _utile_ è sempre seguito da un carattere speciale chiamato **terminatore nullo (`\0`)**.

- Questo `\0` dice a tutte le funzioni (come `cout`, `strlen`, `strcpy`) "La stringa finisce qui".
    
- Se lo dimentichi o lo perdi, causi un disastro (buffer overflow), perché le funzioni continueranno a leggere la memoria che non appartiene alla tua stringa.


---
### Lunghezza Fisica vs. Logica.  

- **Fisica:** La dimensione totale dell'array, quanto _può_ contenere. `char s[50];` // Lunghezza Fisica = 50
    
- **Logica:** Quanti caratteri ci sono _prima_ del `\0`. `strcpy(s, "Ciao");` // Lunghezza Logica = 4 (C-i-a-o).
    
    - In memoria, `s` contiene: `{'C', 'i', 'a', 'o', '\0', ...altri 45 caratteri spazzatura...}`

C++
```cpp
char cstr2[] = {'c','i','a','o', '\0'};  
cout << cstr2 << endl;  
cout << sizeof(cstr2) << endl;  // l. FISICA = 5
cout << strlen(cstr2) << endl;  // l. LOGICA = 4
```
Se imposto io la lunghezza (es. char cstr2[10]) la lunghezza fisica sarà 10.

---

##### FUNZIONE lung. LOCIGA (fuori blocco main)
```cpp
int my_strlen(char cstr[]) {  
    int i = 0;  
    while (cstr[i] != '\0') {  
        i++;    // Incrementa il contatore finché non trova la fine  
    }  
    return i;  // Restituisce il numero di caratteri contati
```
OPPURE: 
```cpp
// Per i vettori generici, la "Lunghezza Logica" (n_usati)
// DEVE essere passata come parametro.
// Anche la Fisica (n_allocati) deve essere passata se serve.
void stampaInfoVettore(const int vettore[], int n_usati, int n_allocati) { 
	cout << " (Dentro la funzione)" << endl;
	
// 1. LUNGHEZZA LOGICA
// È semplicemente il parametro 'n_usati'
cout << " Lunghezza Logica (elementi usati): " << n_usati << endl;

// 2. LUNGHEZZA FISICA
// È semplicemente il parametro 'n_allocati'
cout << " Lunghezza Fisica (spazio totale): " << n_allocati << endl; 
}

int main() { 
// Lunghezza Fisica: 10 elementi 
	int voti[10]; 
	
// Decidiamo di usarne solo 3 per ora. 
// Questa è la nostra "Lunghezza Logica" 
	voti[0] = 30; 
	voti[1] = 28; 
	voti[2] = 25; 
	int n_logica = 3;
	
	cout << "Lunghezza Logica (main): " << n_logica << endl;
	int n_fisica = sizeof(voti) / sizeof(voti[0]);
	cout << "Lunghezza Fisica (main): " << n_fisica << endl;
	cout << "\n--- Chiamo la funzione ---" << endl;
	
	// Passaimo enrambe le funzioni:
	stampaInfoVettore(voti, n_logica, n_fisica);
}
```

---
### Funzione per il cout:
```cpp
//FUNZIONE PER IL COUT  
void my_cout (char cstr[]){  
    int i = 0;  
    while (cstr[i] != '\0'){  
        cout << cstr[i];  
        ++i;  
    }  
    cout << endl;  
}
```

---
## Lettura da Tastiera: `cin >> cstr`

- **Cosa fa:** Legge una parola da `cin` e la memorizza in un array di `char`. Aggiunge automaticamente il `\0` alla fine.
    
- **⚠️ Attenzione (per il lab):** `cin >>` **si ferma al primo spazio**. Se l'utente deve inserire "Marco Cococcioni", e tu usi:

C++
```cpp
char nome[100];
    cout << "Inserisci nome e cognome: ";
    cin >> nome; 
    // L'utente scrive "Marco Cococcioni"
    // 'nome' conterrà solo "Marco" (con \0 dopo la 'o').
    // "Cococcioni" rimane nel buffer di input e creerà problemi alla prossima 'cin'.
```

- **Soluzione (per il lab):** Se devi leggere un'intera riga, usa `cin.getline()`:

C++
```cpp
char nome[100];
cout << "Inserisci nome e cognome: ";
cin.getline(nome, 100); // Legge max 99 caratteri (o fino a 'Invio') e aggiunge \0
```

---
---

### Le 4 Funzioni Magiche (da `<cstring>`)

Per il laboratorio devi sapere a memoria cosa fanno e i loro _pericoli_.
La libreria `<cstring>` fornisce le funzioni fondamentali per manipolare le **c-stringhe**, cioè i vettori di `char` che terminano con `\0`.

- Il concetto **fondamentale** da capire è che tutte queste funzioni non sanno assolutamente nulla della "lunghezza fisica" dei tuoi array. Lavorano tutte seguendo una sola regola:

 	**"Vai avanti finché non incontri il terminatore nullo (`\0`)."**

Questo le rende veloci, ma anche molto pericolose se non sei tu a gestire correttamente la memoria.


1. **`strlen(const char* str)`** (String Length)
    
    - **Cosa fa:** Restituisce la lunghezza _logica_ (non conta il `\0`).
        
    - `strlen("Ciao")` restituisce `4`.

C++
```cpp
int main() {
    char s[] = "Ciao"; // In memoria: {'C', 'i', 'a', 'o', '\0'}
    cout << strlen(s) << endl; // Stampa 4
    return 0;
}
```



2. **`strcpy(char* dest, const char* src)`** (String Copy)
    
    - **Cosa fa:** Copia la stringa `src` dentro `dest`, _incluso_ il `\0`.
        
    - **⚠️ Pericolo:** Non controlla la dimensione di `dest`. Se `src` è più lungo di `dest`, scriverai fuori dall'array (disastro!).
        
    - _Uso corretto:_ `char nome[10] = "Pippo"; strcpy(nome, "Pluto");` // OK
        
    - _Uso errato:_ `char nome[5]; strcpy(nome, "Geronimo");` // BOOM!

C++
 ```cpp
char sorgente[] = "Pluto"; // 5 caratteri + \0 = 6
char destinazione[10];      // Spazio sufficiente (10)

strcpy(destinazione, sorgente);
// Ora 'destinazione' contiene "Pluto"

char destinazione_piccola[4];
// strcpy(destinazione_piccola, sorgente); // SBAGLIATO! "Pluto" (\0) richiede 6 char, ne hai solo 4.
```



3. **`strcat(char* dest, const char* src)`** (String Concatenate)

	- **Cosa fa:** "Appende" (concatena) `src` alla fine di `dest`. Trova il `\0` di `dest`, lo rimpiazza con il primo carattere di `src`e continua.

	- **⚠️ Pericolo:** Come `strcpy`. Devi essere sicuro che `dest` sia abbastanza grande da contenere _entrambe_ le stringhe.

	- _Uso corretto:_ `char saluto[20] = "Ciao "; strcat(saluto, "Mondo");` // saluto ora è "Ciao Mondo"


C++
```cpp
char saluto[20] = "Ciao "; // Spazio fisico di 20
char nome[] = "Mondo";

strcat(saluto, nome);
// Ora 'saluto' contiene "Ciao Mondo"
// Spazio usato: strlen("Ciao ") + strlen("Mondo") + 1 = 6 + 5 + 1 = 12. (OK, 12 < 20)
```



4. **`strcmp(const char* str1, const char* str2)`** (String Compare)
    
    - **Cosa fa:** Confronta le due stringhe in ordine alfabetico (lessicografico).
        
    - **Output (Attenzione!):**
        
        - Restituisce `0` se `str1` e `str2` sono **identiche**.
            
        - Restituisce un numero `< 0` (es. -1) se `str1` viene _prima_ di `str2` (es. "A" e "B").
            
        - Restituisce un numero `> 0` (es. 1) se `str1` viene _dopo_ di `str2` (es. "B" e "A").
            
    - _Errore comune:_ `if (strcmp(s1, s2))` è SBAGLIATO per controllare l'uguaglianza.
        
    - _Uso corretto:_ `if (strcmp(s1, s2) == 0)` // Significa: "le stringhe sono uguali"

ESEMPIO GIUSTO E SBAGLIATO:
```cpp
char s1[] = "Pippo";
char s2[] = "Pippo";

// if (s1 == s2) { ... } // SBAGLIATO! Questo confronta gli indirizzi di memoria, non il contenuto.

// USO CORRETTO per vedere se sono uguali:
if (strcmp(s1, s2) == 0) {
    cout << "Le stringhe sono UGUALI" << endl;
}

char s3[] = "Apple";
char s4[] = "Banana";

if (strcmp(s3, s4) < 0) {
    cout << "Apple viene prima di Banana" << endl;
}
```


---
---

### Le stesse funzioni si possono fare [[sett.4#1. Variabili Globali|fuori dal blocco main]]:
Sostituiremo le 4 funzioni principali:

1. `strlen` -> `my_strlen`
    
2. `strcpy` -> `my_strcpy`
    
3. `strcat` -> `my_strcat`
    
4. `strcmp` -> `my_strcmp`

### 1. `my_strlen` (sostituto di `strlen`)

Riprendo la tua funzione che era perfetta.

- **Scopo:** Restituire la lunghezza _logica_ della stringa (quanti caratteri ci sono _prima_ del `\0`).
    
- **Logica:** Scorre l'array con un contatore `i` e si ferma non appena trova il `\0`. Restituisce il contatore.

C++
```cpp
/*
 * Calcola la lunghezza logica di una cstringa.
 * (Non conta il terminatore \0)
 */
int my_strlen(const char cstr[]) {
    int i = 0;
    while (cstr[i] != '\0') {
        i++;
    }
    return i;
```
*Nota*: ho aggiunto `const` a `char cstr[]` perché questa funzione **promette** di non modificare la stringa, la legge soltanto. È una buona pratica).

---
### 2. `my_strcpy` (sostituto di `strcpy`)

Anche questa ripresa dalla tua, che era corretta.

- **Scopo:** Copiare tutti i caratteri dalla stringa `sorg` alla stringa `dest`.
    
- **Logica:** Copia ogni carattere `sorg[i]` in `dest[i]`. Si ferma quando `sorg[i]` è `\0`. **Fondamentale:** Aggiunge manualmente il `\0` alla fine di `dest`.
    
- **Pericolo:** Come l'originale, questa funzione è pericolosa. Non sa quanto è grande `dest` e si fida che tu gli abbia dato abbastanza spazio.
C++
```cpp
/*
 * Copia la stringa 'sorg' in 'dest'.
 * ATTENZIONE: 'dest' deve essere abbastanza grande!
 */
void my_strcpy(char dest[], const char sorg[]) {
    int i = 0;
    while (sorg[i] != '\0') {
        dest[i] = sorg[i];
        i++;
    }
    // Finito il ciclo, 'i' è sulla posizione DOPO l'ultimo carattere.
    // Dobbiamo aggiungere il terminatore nullo!
    dest[i] = '\0'; 
```


---
### 3. `my_strcat` (sostituto di `strcat`)

Questa è la re-implementazione di `strcat` (concatenazione).

- **Scopo:** "Appendere" la stringa `sorg` alla fine della stringa `dest`.
    
- **Logica:** È un processo in due fasi:
    
    1. Trova la fine di `dest` (cerca il suo `\0`).
        
    2. Inizia a copiare `sorg` _a partire da quella posizione_.
        
- **Pericolo:** Ancora più pericolosa di `strcpy`, perché `dest` deve essere abbastanza grande da contenere _la sua vecchia stringa + la nuova stringa sorg_.

C++
```cpp
/*
 * Concatena (appende) la stringa 'sorg' alla fine di 'dest'.
 * ATTENZIONE: 'dest' deve essere abbastanza grande da contenere
 * la sua vecchia stringa + la nuova!
 */
void my_strcat(char dest[], const char sorg[]) {
    // FASE 1: Trova la fine di 'dest'.
    int i_dest = 0;
    while (dest[i_dest] != '\0') {
        i_dest++;
    }
    // Ora 'i_dest' è l'indice del terminatore \0 di 'dest'

    // FASE 2: Copia 'sorg' a partire da quella posizione.
    int i_sorg = 0;
    while (sorg[i_sorg] != '\0') {
        dest[i_dest] = sorg[i_sorg];
        i_dest++;
        i_sorg++;
    }
    
    // FASE 3: Aggiungi il nuovo terminatore alla fine di tutto.
    dest[i_dest] = '\0';
}
```


---
### 4. `my_strcmp` (sostituto di `strcmp`)

Questa è la re-implementazione di `strcmp` (comparazione).

- **Scopo:** Confrontare `s1` e `s2` in ordine alfabetico.
    
- **Logica:** Scorre entrambe le stringhe _contemporaneamente_. Si ferma alla prima differenza o quando entrambe finiscono.
    
- **Valori di Ritorno:**
    
    - `0` se sono identiche.
        
    - `< 0` (negativo) se `s1` viene prima di `s2`.
        
    - `> 0` (positivo) se `s1` viene dopo di `s2`.
C++
```cpp
/*
 * Confronta s1 e s2 in ordine alfabetico (lessicografico).
 * Restituisce:
 * 0  se s1 == s2
 * < 0 se s1 < s2
 * > 0 se s1 > s2
 */
int my_strcmp(const char s1[], const char s2[]) {
    int i = 0;

    // Cicla finché i caratteri sono UGUALI e
    // non siamo ancora alla fine di NESSUNA delle due stringhe.
    while (s1[i] == s2[i] && s1[i] != '\0' && s2[i] != '\0') {
        i++;
    }

    // Quando usciamo dal ciclo, siamo nel primo punto in cui
    // i caratteri sono diversi, o una stringa è finita.
    //
    // La sottrazione dei codici ASCII ci dà
    // 0 se sono entrambi '\0' (erano uguali)
    // < 0 se s1[i] < s2[i] (es. 'A' - 'B' o '\0' - 'A')
    // > 0 se s1[i] > s2[i] (es. 'B' - 'A' o 'A' - '\0')
    return s1[i] - s2[i];

```
---
 ---


# 2. `typedef` e `const` ([[sett.5|Puntatori]] e Vettori)

**`typedef`**

- **Cosa fa:** Crea un "alias" (un sinonimo) per un tipo di dato.
    
- **Perché:** Rende il codice più leggibile.
    
- **Esempio:** `typedef int Voto;`
    
    - Da ora in poi, puoi scrivere `Voto v = 30;` invece di `int v = 30;`.
        
    - Sembra inutile per `int`, ma diventa comodo con tipi complessi (es. `typedef Studente* PuntatoreStudente;`).

C++
```cpp
// Definisco l'alias: 'Voto' è ora un sinonimo di 'int'
typedef int Voto;

int main() {
    // Uso Voto come se fosse int
    Voto voto_analisi = 30;
    Voto voto_fisica = 28;
    
    std::cout << "Media: " << (voto_analisi + voto_fisica) / 2.0 << std::endl;
    return 0;
```


ESEMPIO CHE SERVE DAVVERO: 
C++
```cpp
// Definisco 'VettoreVenti' come alias per "un array di 20 interi"
typedef int VettoreVenti[20];

// Questa funzione ora è molto più chiara
void stampaReport(VettoreVenti report) {
    std::cout << "Report[0]: " << report[0] << std::endl;
}

int main() {
    // Dichiaro la variabile usando l'alias
    VettoreVenti report_vendite = {100, 50, /* ...altri 18 valori... */};
    
    stampaReport(report_vendite);
    return 0;
```




**`const` con i Vettori (FONDAMENTALE)**

- Quando passi un vettore a una funzione (come `stampa(v, n)`), stai passando il suo indirizzo di memoria. La funzione _può_ modificarlo.
    
- Se una funzione (come `stampa`) deve **solo leggere** il vettore e **non modificarlo**, devi usare `const` per proteggerlo.

C++
```cpp
// Questa funzione PROMETTE di non modificare v
void stampa(const int v[], int n) {
    // v[0] = 99; // ERRORE DI COMPILAZIONE! Ottimo.
    cout << v[0];
}

// Questa funzione PUÒ modificare v
void azzera(int v[], int n) {
    v[0] = 0; // Lecito
}
```

- **Regola per il lab:** Se scrivi una funzione che non deve modificare un vettore, **metti `const`**.


**Puntatori e `const` (La parte difficile)** Leggi la dichiarazione "al contrario" (da destra a sinistra) per capire:

1. **Puntatore a Costante** (`const int* p` oppure `int const* p`)
    
    - Significato: "Puntatore a un intero costante".
        
    - Leggi: `p` è un puntatore a un `int` che è `const`.
        
    - **Restrizione:** Non puoi modificare il _valore_ puntato: `*p = 10;` // NO!
        
    - **Libertà:** Puoi modificare _dove_ punta `p`: `p++;` // OK!
        
    - _Uso:_ Per passare un puntatore a una funzione che può leggere ma non scrivere.
        
2. **Puntatore Costante** (`int* const p`)
    
    - Significato: "Puntatore costante a un intero".
        
    - Leggi: `p` è un puntatore `const` (costante) a un `int`.
        
    - **Restrizione:** Non puoi modificare _dove_ punta `p`: `p++;` // NO! Deve puntare sempre lì.
        
    - **Libertà:** Puoi modificare il _valore_ puntato: `*p = 10;` // OK!
        
    - _Uso:_ Meno comune, serve per avere un puntatore "fisso".
        

---

# 3. Algoritmi di Ordinamento (Sorting)

Per il lab, devi capire la _logica_ dei due cicli annidati.

**rendono i dati _utili_ e la ricerca _velocissima_**.

1. **Per la Ricerca (Il Motivo Principale):**
    
    - **Problema:** Hai un vettore di 1 milione di numeri. Trova se il numero `42` è presente.
        
    - **Senza ordinamento:** Devi usare la **Ricerca Lineare**. Controlli ogni elemento, uno per uno. Nel caso peggiore, fai 1 milione di controlli. (Lento: O(n))
        
    - **Con l'ordinamento:** Se il vettore è ordinato, puoi usare la **Ricerca Binaria**. Controlli l'elemento centrale, "butti via" metà del vettore e ripeti. Trovi il numero in circa 20 controlli. (Velocissimo: O(logn))
        
    - **Analogia:** È la differenza tra cercare un nome in un elenco telefonico (ordinato) e cercare lo stesso nome in una pila di fogli buttati a caso (non ordinato).
        
2. **Per l'Analisi dei Dati:**
    
    - **Trovare il Minimo e il Massimo:** In un vettore ordinato, sono il primo (`v[0]`) e l'ultimo (`v[n-1]`) elemento.
        
    - **Trovare Duplicati:** In un vettore ordinato, i duplicati sono tutti uno accanto all'altro.
        
    - **Calcolare la Mediana:** È l'elemento centrale del vettore ordinato.
        
3. **Per la Presentazione:**
    
    - Mostrare una classifica (dal punteggio più alto al più basso).
        
    - Stampare un elenco di nomi in ordine alfabetico.



### Selection Sort (Ordinamento per Selezione)

- **Idea:** "Trova il più piccolo e mettilo al suo posto".
    
- **Come funziona:**
    
    1. Il ciclo esterno `i` (da 0 a n-2) decide quale "posto" stiamo riempiendo.
        
    2. Il ciclo interno `j` (da `i+1` a n-1) **cerca l'indice** (`i_min`) dell'elemento più piccolo nel _resto_ del vettore (da `i` in poi).
        
    3. Dopo il ciclo interno, abbiamo trovato il minimo. **Scambiamo** `v[i]` con `v[i_min]`.
        
    4. Si ripete. Alla prima passata, il più piccolo va in posizione 0. Alla seconda, il secondo più piccolo va in posizione 1, ecc.
    
    
    Questa funzione implementa l'idea "trova il più piccolo e scambialo al posto `i`".

C++
```cpp
/*
 * Ordina un vettore v di n elementi
 * usando l'algoritmo Selection Sort.
 */
template <typename T>
void selectionSort(T v[], int n) {
    // Ciclo esterno: 'i' è il posto che stiamo riempiendo
    for (int i = 0; i < n - 1; i++) {
        
        // FASE 1: Cerca l'indice del minimo nel resto del vettore
        int i_min = i; // Partiamo assumendo che 'i' sia il minimo
        
        // Ciclo interno: 'j' cerca un elemento più piccolo
        for (int j = i + 1; j < n; j++) {
            // Se troviamo un elemento più piccolo...
            if (v[j] < v[i_min]) {
                // ...ci salviamo il suo indice
                i_min = j;
            }
        }
        
        // FASE 2: Scambia
        // Trovato il minimo (in posizione i_min), lo scambiamo
        // con l'elemento in posizione 'i'
        // (Nota: se i == i_min, scambia sé stesso, non è un problema)
        scambia(v, i, i_min);
        
        // Alla fine di questo ciclo 'for', l'elemento v[i]
        // è al suo posto definitivo.
    }
```


---
### Bubble Sort (Ordinamento a Bolle)

- **Idea:** "Confronta elementi vicini e scambiali se sono nell'ordine sbagliato".
    
- **Come funziona:**
    
    1. Il ciclo esterno `i` serve solo a _ripetere_ il processo.
        
    2. Il ciclo interno `j` (da 0 a `n-1-i`) confronta le coppie adiacenti: `v[j]` e `v[j+1]`.
        
    3. Se `v[j] > v[j+1]`, li **scambia**.
        
    4. Alla fine della prima passata del ciclo interno, l'elemento più _grande_ è "salito" (come una bolla) fino all'ultima posizione.
        
    5. Si ripete, ma il ciclo interno si accorcia ogni volta (per questo `n-1-i`).


Questa funzione implementa l'idea "confronta le coppie vicine e fai 'salire' il più grande"
C++
```cpp
/*
 * Ordina un vettore v di n elementi
 * usando l'algoritmo Bubble Sort.
 */
template <typename T>
void bubbleSort(T v[], int n) {
    // Ciclo esterno: serve a ripetere le passate
    // A ogni passata 'i', l'elemento i-esimo più grande
    // va al suo posto, quindi possiamo accorciare il ciclo interno.
    for (int i = 0; i < n - 1; i++) {
        
        // Ciclo interno: 'j' confronta le coppie adiacenti
        // Si ferma a 'n - 1 - i' perché gli ultimi 'i' elementi
        // sono GIA' al loro posto.
        for (int j = 0; j < n - 1 - i; j++) {
            
            // Confrontiamo l'elemento 'j' con il suo vicino 'j+1'
            if (v[j] > v[j+1]) {
                // Se sono nell'ordine sbagliato, scambiali
                scambia(v, j, j + 1);
            }
        }
        
        // Alla fine di questo ciclo 'for', l'elemento più grande
        // tra quelli analizzati è "salito" (come una bolla)
        // alla posizione 'n - 1 - i'.
    }
}
```


#### ESEMPIO DELL' USO NEL MAIN:
```cpp
// ... Incolla qui le 4 funzioni (stampa, scambia, selectionSort, bubbleSort) ...


int main() {
    cout << "--- Test Selection Sort ---" << endl;
    int dati_int[] = {64, 25, 12, 22, 11};
    int n1 = 5;
    
    cout << "Vettore originale: ";
    stampa(dati_int, n1);
    
    selectionSort(dati_int, n1); // Ordiniamo!
    
    cout << "Vettore ordinato:  ";
    stampa(dati_int, n1);
    
    cout << "\n--- Test Bubble Sort ---" << endl;
    char dati_char[] = {'Z', 'B', 'H', 'A', 'C'};
    int n2 = 5;
    
    cout << "Vettore originale: ";
    stampa(dati_char, n2);
    
    bubbleSort(dati_char, n2); // Ordiniamo!
    
    cout << "Vettore ordinato:  ";
    stampa(dati_char, n2);

    return 0;
}
```
---

# 4. Algoritmi di Ricerca (Searching)

### Ricerca Lineare (o Sequenziale)

Questa è la ricerca più semplice e intuitiva. È come cercare una figurina specifica in un mazzo sparso sul tavolo: le controlli una per una finché non la trovi.

**Valore Restituito:**

- Se trova l'elemento, restituisce l'**indice** (posizione) in cui l'ha trovato.
    
- Se arriva alla fine del vettore senza trovarlo, restituisce un valore convenzionale per "non trovato", di solito **-1**.
    

#### Esempio di Codice (Ricerca Lineare)

Questa funzione scorre tutto il vettore `v` (di `n` elementi) cercando `valore`.
```cpp
/*
 * Cerca 'valore' nel vettore 'v' (lungo 'n') 
 * usando la ricerca lineare.
 * Funziona su vettori ordinati e non ordinati.
 * * Ritorna:
 * - l'indice (posizione) se trova 'valore'
 * - -1 se 'valore' non è presente
 */
template <typename T>
int ricercaLineare(const T v[], int n, T valore) {
    // Ciclo "ignorante": controlla ogni elemento
    for (int i = 0; i < n; i++) {
        // Se l'elemento corrente è quello che cerchiamo...
        if (v[i] == valore) {
            return i; // ...abbiamo finito! Restituiamo la posizione.
        }
    }
    
    // Se il ciclo 'for' finisce, significa che abbiamo
    // controllato tutto senza trovare nulla.
    return -1; 
}
```

### Ricerca Binaria (o Dicotomica) (FONDAMENTALE)

Questa è la ricerca "intelligente". Funziona **solo ed esclusivamente su vettori ordinati**. È come cercare una parola nel dizionario: apri a metà, vedi se la parola che cerchi viene prima o dopo, e butti via la metà inutile del dizionario.

**Valore Restituito:**

- Se trova l'elemento, restituisce l'**indice** in cui l'ha trovato.
    
- Se "esaurisce" il vettore (cioè `sinistro` supera `destro`), restituisce **-1**.
    

#### Esempio di Codice (Ricerca Binaria)

Questa funzione cerca `valore` nel vettore ordinato `v` (di `n` elementi).
```cpp
/*
 * Cerca 'valore' nel vettore 'v' (lungo 'n') 
 * usando la ricerca binaria.
 * !! FUNZIONA SOLO SE 'v' È GIÀ ORDINATO !!
 * * Ritorna:
 * - l'indice (posizione) se trova 'valore'
 * - -1 se 'valore' non è presente
 */
template <typename T>
int ricercaBinaria(const T v[], int n, T valore) {
    int sinistro = 0;
    int destro = n - 1;
    
    // Continua finché la nostra "fetta" di vettore ha senso
    while (sinistro <= destro) {
        
        // 1. Calcola il centro
        // (si usa questo modo per evitare problemi con numeri molto grandi)
        int medio = sinistro + (destro - sinistro) / 2; 
        
        // 2. Confronta
        if (v[medio] == valore) {
            // Trovato!
            return medio;
        }
        
        // 3. Butta via una metà
        if (valore < v[medio]) {
            // Il valore è più piccolo, quindi deve essere nella metà sinistra
            destro = medio - 1;
        } else {
            // Il valore è più grande, quindi deve essere nella metà destra
            sinistro = medio + 1;
        }
    }
    
    // Se usciamo dal ciclo 'while', significa che
    // 'sinistro' ha superato 'destro' e non abbiamo trovato nulla.
    return -1;
}
```

---

# 5. Complessità e Notazione O-grande

Questo è il "voto" che dai a un algoritmo per la sua efficienza. Misura _come_ il tempo di esecuzione _cresce_ all'aumentare dei dati (`n`).

**Le Classi da Sapere (dalla migliore alla peggiore):**

- **O(1) - Costante:** Sempre veloce, non importa quanto è grande `n`.
    
    - _Esempio:_ Accesso a un array (`v[5]`).
        
- **O(logn) - Logaritmica:** Eccellente. `n` può raddoppiare, il tempo aumenta solo di un po'.
    
    - _Esempio:_ **Ricerca Binaria**.
        
- **O(n) - Lineare:** Buona. Il tempo cresce proporzionalmente a `n`.
    
    - _Esempio:_ **Ricerca Lineare**, `strlen`.
        
- **O(n2) - Quadratica:** Lenta per `n` grandi. Il tempo cresce con il quadrato di `n`. (Se `n` raddoppia, il tempo quadruplica!).
    
    - _Esempio:_ **Selection Sort**, **Bubble Sort** (a causa dei due cicli annidati).
        

**Consigli per il Lab:** Il professore potrebbe chiederti: "Che complessità ha la funzione che hai scritto?".

- Se hai scritto una ricerca lineare: rispondi "O(n)".
    
- Se hai scritto un Bubble Sort: rispondi "O(n2)".
    
- Se hai scritto una ricerca binaria: rispondi "O(logn)".



---


# Le Strutture (struct)

### Cosa sono le Strutture (`struct`)?

Finora hai lavorato con tipi di dato "semplici" (come `int`, `double`, `char`) o con collezioni omogenee (array, dove sono _tutti_ `int`o _tutti_ `char`).

**Problema:** Come gestisci un'entità complessa? Pensa a uno "Studente". Uno studente non è solo un numero (matricola) o solo un testo (nome). È un insieme di _dati diversi_ che appartengono alla stessa entità.

Una **struttura** (`struct`) è un "contenitore" che ti permette di raggruppare variabili di tipo diverso sotto un unico nome.

> **Analogia:** Se `int` è un mattone, `struct` è la "cassaforma" che ti permette di creare un blocco personalizzato (es. uno "Studente") che contiene un mattone `int` (matricola), un mattone `char[]` (nome) e un mattone `double` (media).

---

### 2. Definizione e Utilizzo

Si usano due passaggi:

1. **Definizione (La "Cassaforma"):** Dici al compilatore come è fatto il tuo nuovo tipo. Questa è la "ricetta". Di solito si fa fuori dal `main()`.
    
2. **Istanza (Il "Blocco"):** Crei nel `main()` (o in un'altra funzione) una variabile concreta di quel tipo.
    

C++
```cpp
#include <iostream>
#include <string> // Userò string per semplicità, ma funziona uguale con char[]
using namespace std;

// --- 1. Definizione della STRUTTURA ---
// Sto creando un nuovo TIPO chiamato "Studente"
struct Studente {
    // Questi si chiamano "membri" o "campi" (fields)
    int matricola;
    string nome;
    string cognome;
    double mediaVoti;
}; // <-- Nota il punto e virgola obbligatorio!

int main() {
    // --- 2. Creazione (Istanza) ---
    // Dichiaro una variabile di tipo "Studente", come 
    // se fosse un 'int' o un 'double'.
    Studente s1;

    // --- 3. Accesso ai membri (Selettore di membro) ---
    // Per accedere ai campi interni si usa l'operatore PUNTO (.)
    // Questo operatore si chiama "selettore di membro".
    
    s1.matricola = 12345;
    s1.nome = "Mario";
    s1.cognome = "Rossi";
    s1.mediaVoti = 28.5;

    // Posso anche leggere i valori allo stesso modo
    cout << "Studente: " << s1.nome << " " << s1.cognome << endl;
    cout << "Matricola: " << s1.matricola << endl;

    // Si può anche inizializzare subito (come gli array)
    Studente s2 = {56789, "Anna", "Verdi", 29.0};

    cout << "Studente 2: " << s2.nome << endl;

    return 0;
}
```

---

### 3. Operazioni sulle Strutture

Qui copriamo i punti specifici del tuo programma.

#### A. Copia "Membro a Membro"

Cosa succede se assegni una struttura a un'altra?

`struttura1 = struttura2;`

A differenza degli array (che non puoi copiare con `=`), le strutture **possono essere copiate!** Il C++ esegue una **copia membro a membro**: copia il valore di _ogni singolo campo_ dalla struttura sorgente a quella di destinazione.

C++
```cpp
int main() {
    Studente s1 = {123, "Mario", "Rossi", 28.5};
    
    // Dichiaro s3 vuota
    Studente s3; 
    
    // Copia membro a membro
    s3 = s1; 
    
    // Ora s3 è un clone perfetto di s1
    cout << "Studente 3 (copia): " << s3.nome << endl; // Stampa Mario

    // IMPORTANTE: s3 è una COPIA. Se modifico s3, s1 non cambia.
    s3.nome = "Luigi";
    
    cout << "s3 modificato: " << s3.nome << endl; // Luigi
    cout << "s1 originale: " << s1.nome << endl; // Mario (rimane invariato)
}
```


#### B. Strutture come Argomenti di Funzioni (Passaggio "per Valore")

Proprio come `int` o `double`, quando passi una struttura a una funzione, di default C++ ne crea una **copia** (passaggio per valore).

La funzione lavora sulla copia e **non può modificare l'originale**.

C++
```cpp
#include <iostream>
#include <string>
using namespace std;

struct Studente {
    int matricola;
    string nome;
};

// --- Passaggio per VALORE (Copia) ---
// 's' qui è una COPIA dello studente passato dal main.
void stampaStudente(Studente s) {
    cout << "--- Dentro la funzione ---" << endl;
    cout << "Nome: " << s.nome << endl;
    
    // Modifico la COPIA
    s.nome = "MODIFICATO"; 
    cout << "Nome modificato (nella copia): " << s.nome << endl;
    cout << "--- Fine funzione ---" << endl;
}

int main() {
    Studente s1 = {123, "Mario"};
    
    cout << "Nome nel main (prima): " << s1.nome << endl;
    
    stampaStudente(s1); // Passo s1, la funzione ne riceve una copia
    
    cout << "Nome nel main (dopo): " << s1.nome << endl; // Stampa "Mario"

    return 0;
}
```
**Output:**
```
Nome nel main (prima): Mario
--- Dentro la funzione ---
Nome: Mario
Nome modificato (nella copia): MODIFICATO
--- Fine funzione ---
Nome nel main (dopo): Mario
```

_Come vedi, l'originale `s1` non è stato toccato._

> **Nota per il futuro:** Come per le variabili normali, se vuoi che una funzione modifichi la struttura originale, devi passarla **per puntatore** (`Studente*`) o (meglio) **per riferimento** (`Studente&`).

#### C. Strutture come Valore di Ritorno

Una funzione può anche "restituire" un'intera struttura. È molto comodo per le funzioni "costruttrici".

C++
```cpp
#include <iostream>
#include <string>
using namespace std;

struct Studente {
    int matricola;
    string nome;
};

// Questa funzione "promette" di restituire un oggetto Studente
Studente creaStudente(int m, string n) {
    Studente temp;
    temp.matricola = m;
    temp.nome = n;
    
    return temp; // Restituisco l'intera struttura
}

int main() {
    // "Catturo" la struttura restituita dalla funzione
    Studente s_nuovo = creaStudente(777, "Paolo");
    
    cout << "Nuovo studente: " << s_nuovo.nome << endl; // Stampa Paolo

    return 0;
}
```