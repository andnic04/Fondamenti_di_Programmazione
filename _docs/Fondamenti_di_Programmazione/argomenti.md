# SETT 1
### 1. Teoria: Rappresentazione dei Dati nel Calcolatore

- Introduzione al corso e materiale didattico.
    
- Rappresentazione dei caratteri: la tabella **[[Sett.1#^spiegazione-ascii|ASCII]]**.
    
- Rappresentazione dei numeri naturali: il sistema posizionale (base β).
    
- Algoritmo della Sommatoria (conversione da base β a base 10).
    
- Algoritmo [[Sett.1#^div-mod|DIV&MOD]] (conversione da base 10 a base β).
    
- Tecniche di conversione veloce tra basi potenze di due.


### 2. Teoria: Concetti di Programmazione

- Concetto di **[[Sett.1#^5aba60|variabile]]** (come contenitore di informazioni).
    
- Concetto di **tipo di dato**.
    
- Il tipo di dato [[Sett.1#^3d6636|booleano]] (`bool`).
    
- Operazioni booleane: **AND (`&&`)**, **OR (`||`)**, **NOT (`!`)**.


### 3. Pratica: Laboratorio (Ambiente e C++ Base)

- Utilizzo dell'IDE: creazione di un progetto in **CLion**.
    
- Input/Output (I/O) da console:
    
    - Stampa a video (uso di **`cout`**).
        
    - Lettura da tastiera (uso di **`cin`**).

---
# SETT 2
### 1. Teoria: Strutture di Controllo e Operatori (C++)

- Struttura di un programma C/C++.
    
- Istruzione composta (compound-statement).
    
- Principi della programmazione strutturata e Teorema di [[Sett.2#^fcf509|Bohm-Jacopini]].
    
- Operatori di confronto e logici (uguaglianza `==`, `!=` e relazione `<`, `>`, `>=`, `<=`).
    
- Istruzione di selezione [[Sett.2#Istruzione di selezione `if` e `if-else`|`if` e `if-else`]].
    
- Operatore condizionale ([[Sett.2#Operatore condizionale (ternario)|ternario]]).
    
- I cinque aspetti degli operatori (simbolo, arietà, posizione, associatività, priorità).
    
- Operatori [[Sett.2#Operatori binari e unari|binari e unari]].
    
- Trasformazione di espressioni in istruzioni (expression statement).


### 2. Teoria: Tipi di Dato e Rappresentazione (Numeri Interi)

- Il tipo [[Sett.2#-Il tipo `CHAR` e i suoi letterali|char]] e i suoi letterali.
    
- Operazioni definite sul tipo `char`.
    
- Rappresentazione dei numeri interi (in base 2) nel calcolatore.
    
- Rappresentazione in [[Sett.2#Rappresentazione in Modulo e Segno (M&S)|Modulo e Segno]].
    
- Rappresentazione in **Complemento a Due**.
    
- Algoritmi di codifica e decodifica (per Modulo e Segno e Complemento a Due).
    
- Intervalli di rappresentabilità dei numeri interi.


### 3. Pratica: Esercitazione e Laboratorio

- Esercizi sulla rappresentazione dei numeri interi (Modulo e Segno, Complemento a Due).
    
- Esercizi di laboratorio sugli statement `if` e `if-else`.

---
# SETT 3
### 1. Teoria: Operatori e Tipi (C++)

- [[Sett.3#Conversioni di tipo implicite ed esplicite|Conversioni]] di tipo **implicite** ed **esplicite**.
    
- Operatore di [[Sett.3#Operatore di casting `static_cast`|casting]] `static_cast`.
    
- Operatore di assegnamento (`=`).
    
- Concetto di **valore sinistro (l-value)** e **valore destro (r-value)**.
    
- Operatori di assegnamento composti (es. `+=`, `-=`).
    
- Operatori di [[Sett.3#Incremento e decremento|incremento]] (`++`) e [[Sett.3#Incremento e decremento|decremento]] (`--`), sia **prefissi** che **post-fissi**.


### 2. Teoria: Tipi di Dato e Rappresentazione

- Il tipo [[Sett.3#Tipo unsigned|unsigned]] e i suoi letterali.
    
- Operazioni specifiche per `unsigned` (aritmetiche e **bitwise**: `&`, `|`, `^`, `~`, traslazione).
    
- Rappresentazione dei numeri interi con **bias** (algoritmi e intervallo).
    
- Rappresentazione dei numeri reali in **virgola fissa** (prima parte).


### 3. Teoria: Strutture di Controllo

- L'istruzione ripetitiva [[Sett.3#Ciclo While|(ciclo) while.]]


### 4. Pratica: Laboratorio

- Esercizi sui cicli `while`.
    
- Esercizi sulle operazioni con il tipo `unsigned`.

---
# SETT 4
### 1. Teoria: Variabili e Funzioni (C++)

- [[sett.4#VARIABILI GLOBALI E DI BLOCCO (LOCALI)|Variabili globali e variabili di blocco (locali).]]
    
- Le [[sett.4#Le Costanti (`const`)|costanti]].
    
- Il tipo [[sett.4#Tipo Riferimento (`&`)|riferimento]] (`&`).
    
- Concetto di [[sett.4#Concetto di Funzione in C++|funzione]] in C++.
    
- Funzioni senza argomenti e a ritorno [[sett.4#Funzioni Senza Argomenti e Ritorno. TIPO `void`|void]].
    
- Passaggio di argomenti a funzioni per [[sett.4#Funzioni con Argomenti Passati per Valore|valore]].
    
- Passaggio di argomenti a funzioni per [[sett.4#Funzioni con Argomenti Passati per Riferimento (`&`)|riferimento]].


### 2. Teoria: Strutture di Controllo

- Istruzione ripetitiva [[sett.4#Istruzione `do-while`|do-while]].
    
- Istruzione ripetitiva [[sett.4#Istruzione `for`|for]].
    
- Istruzione [[sett.4#Istruzione `break`|break]] (per cicli `for`, `do`, `while`).
    
- Istruzione [[sett.4#Istruzione `continue` (per ciclo `for`)|continue]] (per ciclo `for`).

- Istruzione [[sett.4#Istruzione `switch`|switch]]


### 3. [[sett.4#TEORIA|Teoria]]: Rappresentazione dei Dati

- Rappresentazione dei numeri reali in **virgola fissa** (seconda parte).
    
- Algoritmo "Parte Frazionaria-Parte Intera".
    
- Rappresentazione dei numeri reali in **virgola mobile**.


### 4. Pratica: Laboratorio

- Esercizi sul costrutto `for`.
    
- Esercizi sulle funzioni.
    
- Esercizi sul costrutto `switch` (Nota: menzionato nel laboratorio ma non nelle lezioni di questa settimana).

---
# SETT 5
### 1. Teoria: Puntatori e Funzioni (C++)

- Le variabili **puntatore** (pointer).
    
- La parola chiave [[sett.5#3. Pericolo Puntatori Non Inizializzati e `nullptr`|nullptr]].
    
- L'operatore di indirezione (o dereferenziazione) (`*`).
    
- Funzioni che ritornano un valore.
    
- Concetto di **effetto principale** ed **effetto collaterale** di una funzione.
    
- [[sett.5#4. Puntatori come Argomenti di Funzioni (Effetto Collaterale)|Passaggio di parametri tramite puntatore per modifica]] (es. `incrementa(int* p)`).
    
- **Argomenti default** per le funzioni.
    
- **Overloading** (sovrapposizione) delle funzioni.


### 2. Teoria: Vettori e Puntatori (C++)

- I **vettori** (array monodimensionali).
    
- Inizializzazione, lettura e stampa di un vettore.
    
- **Aritmetica dei puntatori**.
    
- Utilizzo dei puntatori per operare sui vettori (scansione).
    
- Funzioni che operano su vettori (es. minimo, massimo, posizione minimo, scambio elementi).


### 3. Teoria: Rappresentazione dei Dati

- Approfondimenti sulla rappresentazione in **virgola mobile**.

### 4. Pratica: Laboratorio

- Esercizi sul passaggio di parametri per **riferimento** e tramite **puntatore**.
    
- Esercizi sulla chiamata di funzioni usando puntatori e riferimenti.

---
# SETT 6
### 1. Teoria: Stringhe (C-stringhe)

- Vettori di caratteri: le [[sett.6#1. Le Stringhe C (c-stringhe)|C-stringhe]] (stringhe del linguaggio C).
    
- Concetto di **marca di fine stringa** (`\0`).
    
- Differenza tra [[sett.6#Lunghezza Fisica vs. Logica.|lunghezza fisica]] e [[sett.6#Lunghezza Fisica vs. Logica.|lunghezza logica]].
    
- Funzioni della libreria `cstring`: `strlen`, `strcpy`, `strcmp`, `strcat`.
    
- [[sett.6#Lettura da Tastiera `cin >> cstr`|Lettura di C-stringhe da tastiera]] (es. `cin >> cstr`).


### 2. Teoria: Puntatori Avanzati e `const`

- **Puntatori a costante** (dati non modificabili tramite puntatore).
    
- **Puntatori costanti** (indirizzo non modificabile).
    
- Passaggio di un vettore a funzione con attributo `const`.


### 3. Teoria: Algoritmi e Complessità

- Algoritmi di **ordinamento** di vettori:
    
    - `selectionSort`
        
    - `bubbleSort`
        
- Algoritmi di **ricerca** in un vettore:
    
    - Ricerca lineare (sequenziale).
        
    - Ricerca binaria (su vettore ordinato).
        
- Concetto di **complessità di un algoritmo** e notazione **O-grande (Big-O)**.


### 4. Teoria: Tipi di Dato (C++)

- Creazione di sinonimi di tipo (keyword [[sett.6#2. `typedef` e `const` (Puntatori e Vettori)|typedef]]).
    
- Le **strutture** (`struct`).
    
- Operazioni sulle strutture: copia membro a membro, selettori di membro (`.`).
    
- Passaggio di argomenti di tipo `struct` per valore.
    
- Strutture come valore di ritorno di una funzione.


### 5. Pratica: Laboratorio

- Esercizi su array (vettori).
    
- Esercizi sulle C-stringhe.

---
# SETT 7
### 1. Teoria: Array Multi-dimensionali (Statici)

- Gli **array multi-dimensionali**.
    
- Le **matrici** (array 2D).
    
- Inizializzazione delle matrici.
    
- Due modi diversi di passare una matrice (statica) a una funzione.


### 2. Teoria: Memoria Dinamica e Puntatori Avanzati

- Vettori in **memoria dinamica** (approfondimento).
    
- Funzione di esempio: `estendi vettore`.
    
- Concetto di **puntatore a puntatore** (doppio puntatore).
    
- Allocazione di una **matrice in memoria dinamica**.
    
- Concetto di "Vettore di Vettori" (Direttorio).


### 3. Teoria: Strutture Dati

- Introduzione alle **liste**.


### 4. Pratica: Laboratorio

- Esercizi sulle matrici.
    
- Esercizi sui vettori dinamici.