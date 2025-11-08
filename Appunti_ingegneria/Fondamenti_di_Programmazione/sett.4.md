---
layout: page
title: FDP - Lezione Settimana 4
permalink: /fondamenti/settimana-4/
order: 4
---
### VARIABILI GLOBALI E DI BLOCCO (LOCALI)

In C++, il punto in cui dichiari una variabile determina dove essa può essere "vista" e utilizzata nel programma. Le due categorie principali sono le variabili globali e le variabili locali (o di blocco).

### 1. Variabili Globali

Una variabile si definisce **globale** quando è dichiarata **al di fuori di qualsiasi funzione** (inclusa la funzione `main`).

- **Visibilità (Scope):** È "visibile" e accessibile da _qualsiasi_ parte del tuo file sorgente, dal punto in cui è dichiarata fino alla fine del file. Questo include il `main` e tutte le altre funzioni.
    
- **Durata (Lifetime):** Viene creata all'avvio del programma e "muore" (viene distrutta) solo alla fine del programma.
    
- **Inizializzazione:** Se non la inizializzi esplicitamente, il compilatore la inizializza automaticamente a zero (o al suo equivalente nullo, es. `false` per i `bool`, `0.0` per i `double`).

**Esempio**
C++
```cpp
#include <iostream>

int contatore = 10; // 'contatore' è una variabile globale.

void funzioneA() {
    cout << "Funzione A: contatore = " << contatore << endl;
    contatore = 20; // Modifica la variabile globale
}

void funzioneB() {
    cout << "Funzione B: contatore = " << contatore << endl;
}

int main() {
    cout << "Main (inizio): contatore = " << contatore << endl;
    
    funzioneA();
    
    // Il valore è cambiato perché funzioneA() l'ha modificata
    cout << "Main (dopo A): contatore = " << contatore << endl;
    
    funzioneB(); // Legge il nuovo valore
    
    return 0;
}
```
**Output dell'esempio:**
```
Main (inizio): contatore = 10
Funzione A: contatore = 10
Main (dopo A): contatore = 20
Funzione B: contatore = 20
```

---
### 2. Variabili di Blocco (Locali)

Una variabile si definisce **locale** (o **di blocco**) quando è dichiarata **all'interno di un blocco di codice**. Un blocco è qualsiasi porzione di codice racchiusa tra parentesi graffe `{ ... }`.

Questo include:

- Il corpo di una funzione (es. dentro `main` o `funzioneA`).
    
- Il corpo di un ciclo (`for`, `while`, `do`).
    
- Il corpo di un'istruzione condizionale (`if`, `else`).
    
- Qualsiasi coppia di `{}` messa appositamente per limitare lo scope.


- **Visibilità (Scope):** È "visibile" e accessibile solo _all'interno_ del blocco in cui è stata dichiarata (e nei blocchi annidati al suo interno), dal punto di dichiarazione fino alla parentesi graffa chiusa `}`.

- **Durata (Lifetime):** Viene creata quando il programma "entra" nel blocco e viene distrutta non appena il programma "esce" da quel blocco.

- **Inizializzazione:** Se non la inizializzi esplicitamente, il suo valore è **indeterminato** (contiene "spazzatura", cioè qualsiasi valore si trovasse in quella cella di memoria). Usarla prima di assegnarle un valore porta a un _comportamento indefinito_ (un bug grave).


#### Esempio di Variabile Locale
```cpp
void miaFunzione() {
    int y = 5;      // 'y' è una variabile locale a 'miaFunzione'
    cout << "Dentro miaFunzione: y = " << y << endl;
}

int main() {
    int x = 1;      // 'x' è una variabile locale a 'main'
    cout << "Main: x = " << x << endl;

    cout << y;      // ERRORE DI COMPILAZIONE: 'y' non è visibile qui!
    
    miaFunzione();

    if (x == 1) {
        int z = 10; // 'z' è una variabile locale al blocco 'if'
        cout << "Dentro if: z = " << z << endl;
        cout << "Dentro if: x = " << x << endl; // 'x' è visibile
    }

    // std::cout << z; // ERRORE DI COMPILAZIONE: 'z' è morta qui!

    return 0;
}
```
**Output dell'esempio:**
```
Main: x = 1
Dentro miaFunzione: y = 5
Dentro if: z = 10
Dentro if: x = 1
```

### 3. Conflitto di Nomi (Shadowing)

Cosa succede se una variabile locale ha lo stesso nome di una globale? La variabile locale "nasconde" (fa _shadowing_) quella globale. All'interno del blocco, il nome si riferirà _sempre_ alla variabile locale.
```cpp
int valore = 100; 

void funzione() {
    int valore = 20;     // 'valore' locale, nasconde quella globale
    
    cout << "Funzione (locale): valore = " << valore << endl;  //20
    
    // Se proprio devi, puoi accedere alla globale usando l'operatore ::
    cout << "Funzione (globale): valore = " << ::valore << endl;  //100
}

int main() {
    cout << "Main (globale): valore = " << valore << endl;   //100
    
    funzione();
    
    // 'valore' è ancora quella globale,
    // la modifica in 'funzione' era solo sulla locale
    cout << "Main (globale): valore = " << valore << endl;   //100
    
    return 0;
}
```

---

### Le Costanti (`const`)

Una **costante** è una variabile il cui valore **non può essere modificato** dopo l'inizializzazione (che è richiesta sempre). Si usa la parola chiave `const`.

**Regole principali:**

1. **Immutabile**: Una volta assegnato un valore, non puoi più cambiarlo.
    
2. **Inizializzazione Obbligatoria**: Devi assegnarle un valore _subito_, al momento della dichiarazione.
    
3. **Vantaggi**: Rende il codice più leggibile (es. `PIGRECO` è meglio di `3.14159`) e sicuro (il compilatore ti ferma se provi a modificarla).

```cpp
const double PIGRECO = 3.14159;   //Costante globale

int main() {
    
    const int ORE_IN_UN_GIORNO = 24;  //Costante locale
     
    int giorni = 3;
    int ore_totali = giorni * ORE_IN_UN_GIORNO;
    
    // ORE_IN_UN_GIORNO = 25; // ERRORE DI COMPILAZIONE!
                           // Non puoi modificare una 'const'.

    return 0;
}
```

---


# Tipo Riferimento (`&`)

**Riferimento**:
	- identificatore che individua un oggetto;
	- riferimento default: il nome di un oggetto, quando
	questo è un identificatore.
	- oltre a quello default, si possono definire altri
	riferimenti di un oggetto(sinonimi o alias).

**Tipo riferimento:**
	–possibili identificatori di oggetti di un dato tipo (il
	tipo dell’oggetto determina il tipo del riferimento).
**Dichiarazione di un tipo riferimento e definizione di un
riferimento sono contestuali.

---

Un **riferimento** non è una variabile. È un **alias**, cioè un _secondo nome_ che dai a una variabile già esistente. I soprannomi: "Giuseppe" e "Beppe" possono riferirsi alla stessa identica persona.

- **Sintassi:** Si dichiara usando la ==`&`== (e-commerciale) attaccata al tipo.
    
- **Regola 1:** Deve essere **inizializzato subito**, nel momento in cui lo crei, "agganciandolo" a una variabile esistente. Non può esistere un riferimento "vuoto".
    
- **Regola 2:** Una volta "agganciato", **non può più essere ri-agganciato** a un'altra variabile. Rimarrà l'alias di quella variabile originale per tutta la sua vita.
    
- **Conseguenza:** Qualsiasi operazione tu faccia sul riferimento (leggerlo, modificarlo), la stai in realtà facendo sulla variabile _originale_.

#### Esempio di Riferimento
```cpp
int main() {
    int conto_in_banca = 1000;

    // 2. Creo un riferimento (un alias) chiamato 'bancomat'
    // Si legge: "bancomat è un riferimento a un intero"
    int& bancomat = conto_in_banca;

    cout << "Valore originale (conto): " << conto_in_banca << endl; // 1000
    cout << "Valore alias (bancomat): " << bancomat << endl;        // 1000

    // 3. Modifico il valore usando l'ALIAS
    cout << "\nPrelevo 200 euro dal bancomat..." << endl;
    bancomat = bancomat - 200; // Sto modificando 'bancomat'

    // 4. Controllo l'ORIGINALE
    cout << "Valore originale (conto): " << conto_in_banca << endl; // 800!

    // 5. Modifico l'ORIGINALE
    cout << "\nRicevo uno stipendio sul conto..." << endl;
    conto_in_banca = conto_in_banca + 500; // Sto modificando 'conto_in_banca'

    // 6. Controllo l'ALIAS
    cout << "Valore alias (bancomat): " << bancomat << endl;     // Stampa 1300!

    return 0;
}
```
Come vedi, `conto_in_banca` e `bancomat` sono due nomi diversi per la _stessa identica scatola_ in memoria.


---


### Concetto di Funzione in C++

Una **funzione** è un blocco di codice riutilizzabile a cui dai un nome. È come un "operaio specializzato" che esegue un compito specifico.

La programmazione moderna si basa sull'idea di **scomporre un problema complesso** (es. "gestisci un bancomat") in tanti sotto-problemi semplici (es. "leggi carta", "verifica PIN", "mostra saldo"). Ognuno di questi sotto-problemi è risolto da una funzione.

Usare le funzioni è fondamentale per:

- **Riusabilità:** Se devi stampare un menu 5 volte, scrivi la funzione `stampaMenu()` una volta e la _chiami_ 5 volte. (Principio DRY: "Don't Repeat Yourself").
    
- **Astrazione:** Nascondi la complessità. Quando chiami `sqrt(9)` (radice quadrata), non ti interessa _come_ la calcola, ti fidi che ti dia `3`.
    
- **Modularità:** È più facile trovare un errore in una funzione di 10 righe che in un `main` di 1000 righe.


**Sintassi:** `tipo_ritorno nomeFunzione(tipo_arg1, tipo_arg2, ...);` (Nota il punto e virgola alla fine!)

```cpp
int somma(int a, int b) {
    int risultato_locale = a + b;
    return risultato_locale;
}

void stampaMessaggio(string msg) {
    cout << "--- MESSAGGIO: " << msg << " ---" << endl;
}


int main() {
    
    cout << "Chiamo la funzione 'somma' (definita sopra)..." << endl;

    // --- CHIAMATA ---
    int risultato_main = somma(10, 5);
    
    cout << "Il risultato e': " << risultato_main << endl;  //15
    cout << endl; 

    // --- CHIAMATA ---
    stampaMessaggio("Programma completato.");

    return 0;
}
```

---


### Funzioni Senza Argomenti e Ritorno. TIPO `void`

- **Ritorno `void`:** `void` significa "vuoto". La funzione _non restituisce alcun valore_ al chiamante (al `main`). Il suo scopo non è _calcolare_ qualcosa, ma _fare_ qualcosa (un "effetto collaterale", come stampare a schermo).

- **Senza Argomenti (`()`):** Le parentesi sono vuote. La funzione _non riceve alcun dato in input_ per lavorare. Esegue un compito fisso.


##### `void` come Tipo di Ritorno 

Quando metti `void` _prima_ del nome della funzione (come in `void scriviAsterischi(...)`), stai dicendo al compilatore:

> "Questa funzione **non restituisce nessun valore**. Il suo scopo non è calcolare qualcosa (come un numero o una stringa), ma semplicemente **fare qualcosa**."

È una funzione-azione, non una funzione-calcolo.

- **Esempio di Calcolo (NON void):** `int somma(int a, int b)` deve per forza restituire un `int` (es. `return a + b;`).
    
- **Esempio di Azione (SÌ void):** `void scriviAsterischi(int n)` non restituisce nulla. Esegue solo l'azione di stampare a schermo. Non ha un `return ...;` con un valore.


#### Esempio
Questa funzione fa una sola cosa: stampa un messaggio di benvenuto. Non le serve nessun dato (è sempre lo stesso benvenuto) e non deve restituire nessun calcolo.

```cpp
void scriviAsterischi(int n)
{
    for (int i = 0; i < n; i++)
        cout << "*";
    cout << endl;
}

int main()
{
    int i;
    cout << "Quanti asterischi? ";
    cin >> i;
    
    // Chiama la funzione e le passa 'i' come argomento
    scriviAsterischi(i);
    
    return 0;
}
```

```cpp
const int N = 20;

// La funzione non riceve argomenti
void scriviAsterischi(void) // si può scrivere anche void scriviAsterischi()
{
    for (int i = 0; i < N; i++)
        cout << "*";
    cout << endl;
}

int main()
{
    // Chiama la funzione senza passare argomenti
    scriviAsterischi();
    
    return 0;
}
```


---

### Funzioni con Argomenti Passati per Valore

Questo è il modo più comune di passare dati a una funzione.

- **Con Argomenti (`tipo nome`):** Tra le parentesi `()` ora specifichiamo i "pacchi" che la funzione riceve in input. Questi pacchi si chiamano **argomenti** (o parametri).
    
- **Passaggio "Per Valore" (By Value):** Questa è la parte cruciale. Quando chiami la funzione, la variabile che passi _non_ viene data alla funzione. Viene creata una **COPIA** di quella variabile e la funzione lavora _solo sulla copia_.


Pensa a un modulo cartaceo: non dai all'impiegato i tuoi documenti originali, gli dai delle **fotocopie**. L'impiegato può scarabocchiare, evidenziare e strappare le fotocopie, ma i tuoi documenti originali nel `main` rimangono intatti.

#### Esempio

Creiamo una funzione `calcolaArea` che riceve la base e l'altezza, e stampa l'area.

```cpp
// Ritorno: void (stampa solo, non restituisce)
void calcolaArea(int b, int h) {
    // 'b' e 'h' qui dentro sono COPIE dei valori passati dal main.
    
    int area = b * h;
    cout << "L'area del rettangolo " << b << "x" << h << " e': " << area << endl;

    // Modifico la COPIA
    b = 99; 
    cout << "(Dentro la funzione, 'b' ora vale: " << b << ")" << endl;
}


int main() {
    int lato1 = 10;    // 'lato1' e 'lato2' sono le variabili ORIGINALI
    int lato2 = 5;

    cout << "Valori originali prima della chiamata:" << endl;
    cout << "lato1 = " << lato1 << endl;   // 10
    cout << "lato2 = " << lato2 << endl;   // 5
    cout << "---" << endl;
    
    // --- Chiamata alla funzione ---
    // 1. 'lato1' (10) viene COPIATO in 'b'
    // 2. 'lato2' (5) viene COPIATO in 'h'
    // 3. La funzione 'calcolaArea' viene eseguita
    calcolaArea(lato1, lato2); 
    
    cout << "---" << endl;
    cout << "Valori originali dopo la chiamata:" << endl;
    
    // Gli originali non sono stati toccati!
    cout << "lato1 = " << lato1 << endl; // Stampa ancora 10
    cout << "lato2 = " << lato2 << endl; // Stampa ancora 5

    return 0;
}
```
**Output:**
```
Valori originali prima della chiamata:
lato1 = 10
lato2 = 5
---
L'area del rettangolo 10x5 e': 50
(Dentro la funzione, 'b' ora vale: 99)
---
Valori originali dopo la chiamata:
lato1 = 10
lato2 = 5
```

Come puoi vedere, la modifica a `b = 99` fatta dentro la funzione ha modificato solo la _copia_ `b`, lasciando l'originale `lato1`intatto.

--- 


### Funzioni con Argomenti Passati per Riferimento (`&`)

Quando passi un argomento _per riferimento_, **non stai passando una copia**. Stai passando un **alias** (un riferimento, come il `bancomat` dell'esempio) della variabile originale.

Questo significa che il parametro dentro la funzione e la variabile nel `main` diventano **due nomi per la stessa identica scatola in memoria**.

**Conseguenza Fondamentale:** Qualsiasi modifica fatta al parametro _all'interno_ della funzione **modifica direttamente la variabile originale** nel `main`.

### Come si fa

Si usa la e-commerciale (`&`) nella **definizione dei parametri** della funzione.

- `void miaFunzione(int x)`: `x` è una **copia** (passaggio per valore).
    
- `void miaFunzione(int& x)`: `x` è un **alias** (passaggio per riferimento).

### A cosa serve?

Hai due motivi principali per usarlo:

1. **Per Modificare:** Quando lo scopo della funzione è _proprio_ quello di modificare la variabile del chiamante. (Es. una funzione `resetta()` che deve azzerare un contatore).
    
2. **Per Efficienza:** Per evitare la copia di oggetti "pesanti" (come stringhe lunghe, vettori, ecc.). Se non vuoi modificarli, usi `const&` (come visto prima); se _vuoi_ modificarli, usi `&`.
    

### Esempio C++: Modificare l'originale

Confrontiamo una funzione che _non può_ modificare l'originale (passaggio per valore) con una che _può_ (passaggio per riferimento).

C++
```cpp
// --- 1. Passaggio PER VALORE ---
// 'n' è una COPIA di 'valore_main'
void aggiungiDieci_Valore(int n) {
    n = n + 10;
    cout << "Funzione (Valore): n ora vale " << n << endl;
}

// --- 2. Passaggio PER RIFERIMENTO ---
// 'n' è un ALIAS (un altro nome) per 'valore_main'
void aggiungiDieci_Riferimento(int& n) {
    n = n + 10; // Sto modificando l'originale!
    cout << "Funzione (Riferimento): n ora vale " << n << endl;
}

int main() {
    
    // --- Prova con PASSAGGIO PER VALORE ---
    int valore_main = 20;
    cout << "Main (prima): valore_main = " << valore_main << endl;
    
    aggiungiDieci_Valore(valore_main); // Passo una copia
    
    cout << "Main (dopo): valore_main = " << valore_main << endl; // NON è cambiato
    
    cout << "\n-----------------------------------\n" << endl;

    // --- Prova con PASSAGGIO PER RIFERIMENTO ---
    valore_main = 20; // Resetto il valore
    cout << "Main (prima): valore_main = " << valore_main << endl;
    
    aggiungiDieci_Riferimento(valore_main); // Passo un alias
    
    cout << "Main (dopo): valore_main = " << valore_main << endl; // È CAMBIATO!

    return 0;
}
```
**Output dell'esempio:**
```
Main (prima): valore_main = 20
Funzione (Valore): n ora vale 30
Main (dopo): valore_main = 20

-----------------------------------

Main (prima): valore_main = 20
Funzione (Riferimento): n ora vale 30
Main (dopo): valore_main = 30
```

Solo la funzione `aggiungiDieci_Riferimento` (che usa `int& n`) è stata in grado di alterare permanentemente il valore della variabile `valore_main`.




---
---



### Istruzione `do-while`

Il `do-while` (o _do-statement_) è un ciclo "post-condizionale". La sua caratteristica unica è che **controlla la condizione alla fine** di ogni iterazione, non all'inizio.

Questo significa che il blocco di codice all'interno del `do { ... }` viene **eseguito garantitamente almeno una volta**, anche se la condizione è falsa fin dall'inizio.

**Sintassi:**

C++
```cpp
do {
    // Blocco di istruzioni
    // ...
} while (condizione); // Nota il ';' obbligatorio qui
```

**Come funziona:**

1. **FA (do):** Esegue il blocco di istruzioni.
    
2. **MENTRE (while):** Controlla la `condizione`.
    
3. Se la `condizione` è `true`, torna al punto 1 (FA) ed esegue di nuovo il blocco.
    
4. Se la `condizione` è `false`, esce dal ciclo e prosegue con il resto del programma.
    

**Esempio (Il caso d'uso classico: un menu):** Vuoi mostrare il menu _almeno una volta_ prima di chiedere all'utente se vuole uscire.

C++
```cpp
int main() {
    char scelta;
    
    do {
        cout << "\n--- MENU ---" << endl;
        cout << "1. Gioca" << endl;
        cout << "2. Opzioni" << endl;
        cout << "3. Esci" << endl;
        cout << "Fai la tua scelta: ";
        cin >> scelta;

        // ... qui andrebbe il codice per gestire la scelta ...
        if (scelta == '1') cout << "Avvio del gioco..." << endl;
        
    } while (scelta != '3'); // Ripeti finché l'utente NON sceglie '3'
    
    cout << "Arrivederci!" << endl;
    return 0;
}
```

---

### Istruzione `for`

Il ciclo `for` è l'istruzione "contatore" per eccellenza. È ideale quando sai _a priori_ quante volte vuoi eseguire un blocco di codice (o, più in generale, quando hai un'inizializzazione, una condizione di permanenza e un'azione da fare a fine ciclo).

È compatto perché definisce i tre elementi chiave del ciclo in una sola riga.

**Sintassi:**
C++
```cpp
for (inizializzazione; condizione; incremento) {
    // Blocco di istruzioni
}
```


**Come funziona (questo è cruciale):**

1. **`inizializzazione`**: Viene eseguita **una sola volta** all'inizio (es. `int i = 0;`).
    
2. **`condizione`**: Viene controllata **prima** di ogni iterazione (es. `i < 10;`). Se è `false`, il ciclo termina immediatamente.
    
3. **`Blocco di istruzioni`**: Se la condizione è `true`, il blocco viene eseguito.
    
4. **`incremento`**: Viene eseguito **alla fine** di ogni iterazione (es. `i++`).
    
5. Si torna al punto 2 (controllo condizione).
    

**Esempio (Stampa i numeri da 0 a 4):**

C++
```cpp
int main() {
    // 1. i = 0 (inizializzazione)
    // 2. i < 5 ? (0 < 5 -> true)
    // 3. Esegue cout
    // 4. i++ (i diventa 1)
    // ...
    // 2. i < 5 ? (5 < 5 -> false)
    // 5. Esce dal ciclo
    
    for (int i = 0; i < 5; i++) {
        cout << "Il valore di i e': " << i << endl;
    }
    
    cout << "Ciclo 'for' terminato." << endl;
    return 0;
}
``````

---

### Istruzione `break`

L'istruzione `break` è l'**uscita di emergenza**.

Quando viene incontrata, **interrompe immediatamente** l'esecuzione del ciclo (`for`, `while`, `do-while`) o del costrutto `switch`(come vedremo tra poco) più interno in cui si trova.

L'esecuzione del programma salta subito alla prima istruzione _dopo_ la parentesi graffa `}` di chiusura del ciclo.

**Esempio (Cercare un valore in un ciclo):** Immagina di voler trovare il primo numero maggiore di 10 divisibile per 7. Non ha senso continuare a cercare dopo averlo trovato.

C++
```cpp
#include <iostream>
using namespace std;

int main() {
    int numero_trovato = 0;
    
    // Controlliamo i numeri da 11 a 100
    for (int i = 11; i <= 100; i++) {
        
        if (i % 7 == 0) {
            // Trovato!
            numero_trovato = i;
            break; // Esci IMMEDIATAMENTE dal ciclo 'for'
        }
        
        // Questo non verrà stampato dopo il break
        cout << "Controllo " << i << "..." << endl;
    }
    
    cout << "Il primo numero trovato e': " << numero_trovato << endl;
    return 0;
}
```
**Output:**
```
Controllo 11...
Controllo 12...
Controllo 13...
Il primo numero trovato e': 14
```

(Nota come smette di stampare "Controllo..." dopo il 13, perché il `break` a 14 ha interrotto il ciclo).

---

### Istruzione `continue` (per ciclo `for`)

L'istruzione `continue` è diversa da `break`. Non esce dal ciclo, ma **interrompe solo l'iterazione corrente** e "salta" all'iterazione successiva.

In pratica, dice al ciclo: "Ho finito con questa iterazione, ignora il resto del codice nel blocco e passa alla prossima".

**Nel ciclo `for`**: Quando incontra `continue`, il programma salta subito alla parte di **incremento** (es. `i++`) e poi rivaluta la condizione.

**Esempio (Stampare solo i numeri dispari):** Vogliamo ciclare da 1 a 10, ma _saltare_ la stampa se il numero è pari.

```cpp
#include <iostream>
using namespace std;

int main() {
    for (int i = 1; i <= 10; i++) {
        
        if (i % 2 == 0) {
            // Se 'i' è pari, salta il resto del blocco
            continue; 
            // e vai dritto a i++
        }
        
        // Questo 'cout' viene eseguito solo se 'i' è dispari
        cout << i << " ";
    }
    
    cout << "\nCiclo terminato." << endl;
    return 0;
}
```
**Output:**
```
1 3 5 7 9 
Ciclo terminato.
```

---

### Istruzione `switch`

L'istruzione `switch` è un'alternativa più pulita a una lunga catena di `if ... else if ... else`

Viene usata per **selezionare uno tra molti blocchi di codice** da eseguire, basandosi sul valore di una _singola variabile_(che deve essere di tipo intero, `char` o `enum`).

**Sintassi:**
```cpp
switch (variabile) {
    case valore_costante_1:
        // Codice da eseguire se variabile == valore_costante_1
        break;
        
    case valore_costante_2:
        // Codice da eseguire se variabile == valore_costante_2
        break;
        
    // ... altri case ...
    
    default:
        // Codice da eseguire se NESSUN case corrisponde
        break; // Opzionale, ma buona pratica
}
``````

**Punti chiave:**

1. **`case`**: Può controllare solo valori _costanti_ (es. `5`, `'A'`), non variabili (es. `case x:` non è valido).
    
2. **`break`**: È **FONDAMENTALE**. Se dimentichi il `break` alla fine di un `case`, il programma _continuerà a eseguire_ anche il codice dei `case` successivi (questo si chiama "fall-through").
    
3. **`default`**: È opzionale. Corrisponde all' `else` finale: viene eseguito se la variabile non corrisponde a nessun `case`.


**Esempio (Gestire un voto in lettere):**
```cpp
int main() {
    char voto;
    cout << "Inserisci il tuo voto (A, B, C, D o F): ";
    cin >> voto;
    
    switch (voto) {
        case 'A':
            cout << "Eccellente!" << endl;
            break;
            
        case 'B':
            cout << "Ottimo" << endl;
            break;
            
        case 'C':
            cout << "Buono"s << endl;
            break;
            
        case 'D':
            cout << "Sufficiente" << endl;
            break;
            
        case 'F':
            cout << "Insufficiente" << endl;
            break;
            
        default:
            // Se l'utente scrive 'G', 'a', '5', ecc.
            cout << "Voto non valido." << endl;
            break;
    }
    
    return 0;
}
```


---
---


# TEORIA
### Rappresentazione in Virgola Fissa

È un metodo per rappresentare numeri con la virgola (reali) usando, in realtà, solo numeri interi.

- **Come funziona:** Si **decide a priori** (si "fissa") dove si trova la virgola binaria. Non si memorizza la sua posizione, la si assume e basta.
    
- **Analogia:** È come se decidessi di gestire i soldi lavorando solo in centesimi. Il numero intero `1250` _rappresenta_ `12,50 €`. La virgola è "fissata" a due posti dalla fine, ma non è scritta da nessuna parte.


- **"Seconda Parte" (Parte Frazionaria):** Se in binario hai `1011.0110`, i bit _dopo_ la virgola fissa rappresentano potenze negative di 2:
    
    - $0⋅2^{−1}  (0.5)$
        
    - $1⋅2^{−2} (0.25)$
        
    - $1⋅2^{−3} (0.125)$
        
    - $0⋅2^{−4} (0.0625)$

- **Pro e Contro:** È molto **veloce** (usa la matematica degli interi), ma ha un **range di valori molto limitato** e una precisione fissa.


---

### Algoritmo "Parte Frazionaria-Parte Intera"

Questo è l'algoritmo che si usa per **convertire la parte frazionaria** di un numero (es. `0.625`) da decimale a binario.

Si basa su moltiplicazioni successive per 2:

1. Prendi **solo** la parte frazionaria (es. `0.625`).
    
2. **Moltiplica per 2**.
    
3. La **parte intera** del risultato è il tuo primo bit binario.
    
4. Prendi la **parte frazionaria** del risultato e torna al punto 2.
    
5. Continua finché la parte frazionaria non diventa 0 (o finché non hai abbastanza bit).
    

**Esempio (Convertire 0.625):**

- `0.625 * 2 = 1.25` -> Parte Intera = **1**
    
- `0.25 * 2 = 0.5` -> Parte Intera = **0**
    
- `0.5 * 2 = 1.0` -> Parte Intera = **1**
    
- `0.0` -> Fine.
    

Il risultato si legge dall'alto in basso: `0.625` (decimale) = `0.101` (binario). (Infatti: $1⋅2^{−1} + 0⋅2^{−2} + 1⋅2^{−3} = 0.5 + 0 + 0.125 = 0.625$)

---

###  Rappresentazione in Virgola Mobile (Floating Point)

Questo è il metodo **standard (IEEE 754)** che i computer usano per i tipi `float` e `double`.

- **Concetto chiave:** È la **notazione scientifica** applicata al binario. La virgola non è fissa, ma "fluttua" (si _muove_) in base a un esponente.
    
- **Come funziona:** Un numero non viene memorizzato come `1234.56`, ma come "Parti da `1.23456` e sposta la virgola di 3 posti" (cioè $1.23456×10^3$).


- **Struttura:** Ogni numero `float` o `double` è diviso in 3 parti:
    
    1. **Segno (1 bit):** Positivo (0) o negativo (1).
        
    2. **Esponente:** Dice di _quanto_ spostare la virgola (come il `3` nell'esempio).
        
    3. **Mantissa:** Contiene le cifre significative del numero (come `1.23456`).


- **Pro e Contro:** Ha un **range di valori enorme** (può salvare numeri piccolissimi e grandissimi). Di contro, introduce **errori di arrotondamento**: non tutti i numeri decimali possono essere rappresentati con precisione esatta.