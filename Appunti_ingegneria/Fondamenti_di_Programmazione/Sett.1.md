---
layout: page
title: FDP - Lezione Settimana 1
permalink: /fondamenti/settimana-1/
order: 1
---
### Concetti di Rappresentazione

1. **Rappresentazione Caratteri (Tabella ASCII)** Il computer capisce solo numeri. L'**ASCII** (American Standard Code for Information Interchange) è una tabella che assegna un numero univoco (da 0 a 127) a ogni carattere (lettere, numeri, punteggiatura, simboli). Ad esempio, 'A' è 65, 'B' è 66, 'a' è 97. Questo permette al computer di memorizzare il testo come una sequenza di numeri. ^spiegazione-ascii

2. **Rappresentazione Numeri Naturali (Posizionale)** È il modo in cui scriviamo i numeri. Il valore di una cifra dipende dalla sua **posizione**. In base 10 (decimale), il numero 123 significa (1×102)+(2×101)+(3×100). Questo vale per qualsiasi base (beta): in base 2 (binario), il numero 101 vale (1×22)+(0×21)+(1×20)=4+0+1=5 (in base 10).

3. **Algoritmo DIV&MOD (Cambio di Base)** È la tecnica principale per convertire un numero da una base (es. 10) a un'altra base (es. 2). Si divide ripetutamente il numero per la nuova base e si annotano i **resti** (MOD) in ordine inverso. _Esempio: 13 in base 2_ ^div-mod
    
    - 13 / 2 = 6, Resto **1**
        
    - 6 / 2 = 3, Resto **0**
        
    - 3 / 2 = 1, Resto **1**
        
    - 1 / 2 = 0, Resto **1** 

Leggendo i resti dal basso verso l'alto: 1101 (in base 2).   (13)_dieci_ => (1101)_due_

---
### Concetti di Programmazione (C++)

4. **Variabile e Tipo** ^5aba60
    
    - **Variabile:** È un "contenitore" nella memoria del computer a cui diamo un nome (es. `eta`). Serve a memorizzare un dato (es. 25) che può cambiare durante l'esecuzione del programma.
        
    - **Tipo:** Specifica che tipo di dato la variabile può contenere (es. `int` per numeri interi, `char` per caratteri, `bool` per valori logici). Il tipo dice al computer quanto spazio serve e quali operazioni sono permesse.

5. **Tipo `bool` e Operatori Logici** ^3d6636
    
    - **`bool`**: È un tipo di dato che può avere solo due valori: `true` (vero) o `false` (falso).
        
    - **Operatori Logici:** Combinano o modificano valori `bool`:
        
        - **`&&` (AND):** `a && b` è `true` solo se _entrambi_ `a` e `b` sono `true`.
            
        - **`||` (OR):** `a || b` è `true` se _almeno uno_ tra `a` e `b` è `true`.
            
        - **`!` (NOT):** `!a` inverte il valore: `!true` diventa `false` e `!false` diventa `true`.

6. **`cout` e `cin` (Input/Output)** Sono i comandi base di C++ per comunicare con l'utente:
    
    - **`cout` (Console OUTput):** Stampa dati a schermo (es. testo, valori di variabili). Si usa con `<<` (es. `cout << "Ciao";`).
        
    - **`cin` (Console INput):** Legge dati inseriti dall'utente tramite la tastiera e li memorizza in una variabile. Si usa con `>>` (es. `cin >> eta;`).

7. **CLion** È un **IDE** (Integrated Development Environment), cioè un software che aiuta a scrivere codice. Include un editor di testo, un compilatore (che traduce il codice in linguaggio macchina) e un debugger (per trovare errori). È lo strumento che usi per creare i tuoi "progetti" (programmi).