# Esercizio progressivo: mini-app per la libreria personale

L’esercizio consiste nel realizzare, in modo progressivo, una piccola applicazione per gestire la propria libreria personale usando classi e oggetti (OOP), funzioni e codice procedurale, in ottica Model–View–Controller (MVC).

## Obiettivo

Implementare un sistema che permetta di:

- aggiungere nuovi libri alla libreria personale;
- visualizzare la lista dei libri con le informazioni principali;
- segnare un libro come "letto";
- gestire diversi formati di libro (cartaceo, digitale, audio);
- rimuovere un libro dalla lista.

---

## Paradigma di sviluppo: MVC

Il progetto deve essere sviluppato secondo il modello MVC:

- Model (OOP): classi e oggetti che rappresentano i libri e le loro varianti.
- View (funzionale): funzioni che trasformano i dati (oggetti libro) in HTML (lista, form, pulsanti ecc.).
- Controller (procedurale): codice che decide cosa succede prima e cosa succede dopo, gestendo gli eventi (submit del form, click sui pulsanti, ecc.).

### Mental model da seguire

1. Creare un modello per gli oggetti (libri).
2. Generare oggetti a partire dalle classi.
3. Manipolare questi oggetti per stamparli, memorizzarli e farci operazioni.
4. Decidere l’ordine delle operazioni e le dipendenze (controller).

---

# Model: classi e oggetti

## 1. Classe base Libro

Definire una classe principale `Libro` che contenga almeno:

- `id`
- `titolo`
- `autore`
- `anno`
- `formato` (`cartaceo`, `digitale`, `audio`)
- `letto`, inizialmente impostato a `false`

Questa classe rappresenta la classe madre dalla quale deriveranno le varianti specifiche.

### Incapsulamento

Il docente ha chiarito che l’incapsulamento è un concetto fondamentale: le variabili della classe esistono e possono essere definite e lette esclusivamente all’interno della classe.

Per l’esercizio significa che:

- i campi del libro (inclusi `id`, formato e dimensione nel caso digitale) sono proprietà interne della classe;
- l’accesso a questi dati deve avvenire tramite metodi della classe (costruttore, getter/setter, metodi di utilità), non direttamente dall’esterno;
- la logica che decide quando mostrare il campo `dimensione` o come usare il formato digitale è racchiusa nella struttura delle classi e non dispersa nel resto del codice.

## 2. Sottoclasse LibroDigitale / DigitalBook

Definire una classe `LibroDigitale` (oppure `DigitalBook`) che estende `Libro` tramite ereditarietà.

Aggiungere campi specifici del formato digitale, ad esempio:

- `dimensione`: dimensione del file in MB

Questa sottoclasse rappresenta il caso in cui il libro è in formato digitale e specializza il comportamento della classe madre.

### Ereditarietà

`DigitalBook` estende `Book` e aggiunge campi specifici come la dimensione del file.

### Polimorfismo

La gestione uniforme di oggetti `Book` e `DigitalBook` attraverso la stessa interfaccia (ad esempio una lista di libri oppure lo stesso metodo di rendering) permette al codice di trattare in modo polimorfo i diversi tipi di libro.

La sottoclasse deve personalizzare la visualizzazione HTML rispetto alla classe base, in modo che un libro digitale venga mostrato anche con i dati aggiuntivi (ad esempio dimensione in MB).

## 3. Sottoclasse LibroAudio

Definire anche una classe `LibroAudio`, con la stessa logica di `LibroDigitale`.

La classe deve:

- estendere `Libro`;
- aggiungere campi tipici di un audiolibro, ad esempio durata, narratore, ecc.;
- prevedere eventualmente un override della formattazione o della visualizzazione HTML.

## 4. Metodi di base

Sulla classe `Libro` e/o sulle sottoclassi definire metodi per:

- segnare un libro come letto (`segnaComeLetto()`);
- restituire una rappresentazione HTML del libro (`toHTML()`), utile per la View;
- includere titolo, autore, anno, formato, stato di lettura e pulsanti azione;
- eventuali getter/setter per gestire internamente i dati degli oggetti.

---

# View: form HTML e render della lista

## 1. Form base

Creare un form HTML per inserire nuovi libri. Il form deve includere:

- `id` (se necessario per la gestione interna dell’oggetto)
- titolo;
- autore;
- anno;
- formato (`cartaceo`, `digitale`, `audio`);
- eventuali campi aggiuntivi in base al formato scelto:
  - digitale: `dimensione` in MB;
  - audio: durata, narratore, ecc.
- un bottone `submit` per aggiungere il libro alla lista.

## 2. Gestione dinamica del formato nel form

Il form deve mostrare o nascondere il campo relativo alla dimensione del libro digitale in base al valore scelto nella select (cartaceo/digitale).

Questo collegamento è importante perché riflette la logica delle classi:

- se il formato selezionato è `cartaceo`, si usa la struttura della classe base `Libro`;
- se il formato selezionato è `digitale`, si usa la sottoclasse `DigitalBook`, che richiede la proprietà `dimensione`.

## 3. Render della lista di libri

Implementare una funzione (o più funzioni) che:

- riceve la collezione di oggetti `Libro` e relative sottoclassi;
- genera il corrispondente HTML della lista;
- mostra per ciascun libro:
  - dati principali (titolo, autore, anno, formato);
  - stato di lettura (`Letto` / `Da leggere`);
  - pulsanti azione: `Segna come letto`, `Rimuovi`.

Può essere utile aggiornare la vista con `replaceChildren()` oppure con una tecnica equivalente, quando la lista cambia dopo aggiunta, modifica o rimozione di un libro.

---

# Controller: gestione degli eventi

Il controller è la parte di codice procedurale che gestisce gli eventi e decide l’ordine delle operazioni.

## 1. Eventi principali

Gestire almeno:

### Submit del form

- leggere i valori inseriti dall’utente;
- in base al formato selezionato, istanziare la classe corretta (`Libro`, `LibroDigitale`, `LibroAudio`);
- aggiungere il nuovo oggetto alla collezione di libri;
- richiamare la funzione di render della lista.

### Click sui pulsanti della lista

Usando delegazione degli eventi:

- click su `Segna come letto`: aggiornare la proprietà `letto` dell’oggetto e ridisegnare la lista;
- click su `Rimuovi`: rimuovere il libro dalla collezione e aggiornare la vista.

### Change sui campi del form

Se necessario:

- mostrare o nascondere campi aggiuntivi in base al formato selezionato (digitale / audio).

## 2. Logica procedurale

Il controller deve esplicitare:

- cosa succede prima (lettura dei dati dal form);
- cosa succede dopo (creazione dell’oggetto, aggiornamento della lista);
- chi dipende da chi (per esempio, il render dipende dallo stato aggiornato della collezione di libri).

---

# Funzionalità richieste (riepilogo)

- gestione della libreria personale tramite classi e oggetti;
- classe base `Libro` con campi fondamentali e stato `letto = false` iniziale;
- sottoclassi `LibroDigitale` e `LibroAudio` che estendono `Libro` e aggiungono campi specifici;
- uso corretto di ereditarietà e polimorfismo;
- incapsulamento: campi e dati interni definiti e accessibili tramite metodi della classe;
- form HTML con campi e bottone `submit` per aggiungere nuovi libri;
- mostrare/nascondere i campi del formato digitale in base alla selezione;
- render della lista con stato di lettura e pulsanti azione;
- gestione di `submit`, `change` e `click` (delegato) in ottica controller procedurale;
- applicazione dei tre paradigmi:
  - OOP per il modello;
  - funzionale per la vista;
  - procedurale per il controller.

---

# Deliverable atteso

Realizzare una mini-app che permetta di:

- aggiungere libri della propria libreria;
- visualizzarli in una lista strutturata;
- cambiare lo stato di lettura;
- rimuoverli;
- mostrare correttamente le informazioni specifiche dei formati cartaceo, digitale e audio;
- far funzionare correttamente la logica di form dinamica per il formato digitale;
- rispettare i principi di ereditarietà, polimorfismo e incapsulamento.

L’esercizio deve essere strutturato in modo chiaro secondo il paradigma Model–View–Controller e deve dimostrare l’uso integrato dei tre paradigmi di programmazione richiesti.
