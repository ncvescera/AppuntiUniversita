# Basi di Dati

## Indice

* [Introduzione](#introduzione)
   * [DB](#db)
   * [DBMS](#dbms)
* [Modello dei Dati](#modello-dei-dati)
* [Modello Relazionale](#modello-relazionale)
    * [Definizioni](#definizioni)
* [Algebra Relazionale](#algebra-relazionale)
* [Calcolo Relazionale](#calcolo-relazionale)
* [Dipendenze Funzionali ANALI](#dipendenze-funzionali-anali)
* [Forme Normali](#forme-normali)

## Introduzione

### DB

**Definizione basi di dati:** collezione di dati correlati <br>
**Definizione dati:** sono fatti noti che possono essere memorizzati aventi un significato implicito

Una base di dati (DB) ha le seguenti proprietà implicite:

* Rappresenta un certo aspetto del mondo reale. Se cambia il mondo cambiano i dati
* È una collezione di dati logicamente coerenti
* È progettata e costruita per uno scopo specifico dettato dagli interessi degli utenti finali

Viene gestita tramite un software chiamato DBMS (DataBase Management System)

![db_schema](./imgs/db_schema.png)

### DBMS

È il software che gestisce la basi di dati ed ha le seguenti funzioni:

* Definire il DB indicando i tipi di dati, struttura e vincoli
* Costruire il DataBase 
* Manipolare il DB con le seguenti operazioni:
    * Interrogare il DB per reperire dati
    * Aggiornare il DB per ripescchiare i cambiamenti nel minimondo
    * Accedere al DB attraverso applicazioni Web
* Condividere il DB, permettendo ad utenti e applicazioni di accedervi senza violare la consistenza dei dati (Florindi Approva)

Inoltre al DBMS sono delegate le funzioni di:

* Protezione e Manutenzione del DB
* Processing attivo dei dati (se il DBMS è attivo)
* Funzioni di presetnazione e visualizzazione dei dati

Alcuni vantaggi nell'utilizzo di un DBMS rispetto a semilici files sono:

* La sua natura autodescrittiva:
    * nei DB viene memorizzata una tabella che contiene struttura, vincoli e descrizioen complenta dei dati (viene detta catalogo e contiene i Metadati relativi al DB).
    * Permettono ai DBMS di accedere a diverse basi di dati con più facilità ![catalogo](./imgs/catalogo.png)
* La separazione tra programmi e dati e conseguentemente la loro astrazione:
    * agli utenti viene fornita una rappresentazione semplificata dei dati, detta Modello dei Dati.
    * le applicazioni si riferiscono al modello logico piuttosto che all'effettiva memorizzazione dei dati (memorizzata nel catalogo)
    * grazie a questa separazione è possibile modificare al struttura dei dati senza modificare i programmi di accesso ad essa
* Supporto di viste multiple e di condivisione sicura di dati in ambiente multiutente:
    * ogni utente può richiedere la visione di un diverso sottoinseme del DB, o di un insieme di dati non esplicitamente memorizzati, che prende il nome di vista
    * poichè un DBMS multiutente deve gestire più accessi contemporaneamemte deve anche tener conto della concorrenzialità delle modifiche che vengono effettuate al DB, garantendo l'**isolamento** e l'**atomicità** delle transazioni che vengono effettuate sul DB

#### Utenti di un DBMS

Ci sono 2 principali tipi di utenti:

* Attori in scena:
    * interagiscono direttametne con in DB in base alle loro compenteze
* Attori dietro le quinte:
    * progettano il DBMS ma non hanno interesse nelle Basi di Dati

#### Vantaggi di un DBMS

* Controllo della ridondanza:
    * permette di evitare la ridondanza o permette di avere casi di ridondanza controllata che non va ad influire sulla consisntenza dei dati
* Divieto di accesso non autorizzato:
    * fornisce un sistema per la sicurezza e l'autorizzazione all'accesso del DB (viene gestita dal DBA)
* Strutture di memorizzazione efficienti per l'eseuzione efficiente di query:
    * il DBMS utilizza indici (strutture ad albero o tabelle hash) per effettuare ricerche più veloci sui dati salvati su disco
    * impiego di un buffer che mantiene porzioni di DB nella memoria principale
* Backup e Recovery:
    * sistema per il ripristino del DBMS in caso di guasto
* Interfaccia utente:
    * fornisce varie interfaccie per faciliterne l'utilizzo
* Rappresentazioni di associazioni complesse tra i dati:
* Impostazione di Vincoli di Integrità
* Permesso di eseguire Inferenze e azioni tramite regole:
    * possibilità di definire regole per l'aggiornamento dei dati (per i DB attivi queste regole possono essere anche automatiche)
* Flessibilità:
    * possibilità di alterare la sturttura del DB senza interagire con i dati memorizzati al suo interno

In alcuni casi può essere svantaggioso utilizzare un DBMS rispetto ad una tradizionale gestione dei files:

* Basi di dati ed applicazioni semplici e ben definite
* L'accesso ai dati deve essere estremamente veloce e non si può perdere tempo con eventuali elaborazioni da parte del DMBS
* Non vi è accesso multiutente

## Modello dei Dati

Il modello dei dati è un isnieme che descrive la struttura (tipi di dato, associazioni tra i dati e i vincoli) di un DB e le operazioni per manipolare i suoi dati.<br>
Oltre alle operazioni di base (selezione, aggiornamento e cancellazione) il modello può anche contenere concetti per la specifica dell'aspetto dinamico di un DB.
I modelli dei dati possono essere di vari tipi:

* Alto Livello: forniscono concetti compendibili dagli utenti finali
* Basso Livello: forniscono dettagli sulla memorizzazione fisica dei dati
* Implementabili: forniscono concetti comprensibili dagli utenti ma con dettagli aggiunti che ne permettono implementazione diretta sul calcolatore

### Schema Vs Istanza

In una base di dati bisogna fare una distinzione tra Schemi e Istanze:

* Lo Schema (intenzione) rappresenta una descrizione del DB e viene specificato durante la fase di progettazione, può anche essere rappresentato graficamente tramite un diagramma di shcema. Serve per rappresentare i campi che ogni tabella del DB contiene. Generalmente cambia poco frequentemente. ![intenzione](./imgs/intenzione.png)
* L'Istanza (estenzione) rappresenta il contenuto del DB in un certo istante di tempo. Varia ad ogni aggiornamento del DB.

### Architettura a 3 Livelli

Per una migliroe implementazione dei DBMS è stata introdotta un'architettua a 3 Livelli utile supportare l'Indipendenza dei Dati e le Viste Multiple:

1. Schema Interno: serve per descrivere al memorizzazione fisica dei dati e delle strutture di accesso
2. Schema Conettuale: serve per desrivere le strutture e i vincoli del DB
3. Schema Esterno: descrive le varie viste dell'utente

![3livelli](./imgs/3livelli.png)

### Indipendena dei Dati

L'indipendenza dei dati può essere:

* Logica: à la capacità di effettuare cambiamenti al livello Concettuale senza alterare il livello Esterno.
* Fisica: è la capacità di modificare il livello interno tenendo inalterato il livello Concettuale.

Questo consente di dover solamente modifcare i mapping tra i veri livelli che sono cambiati.

## Linguaggi

Vengono definiti 2 tipi di lingauggi:

* DDL (Data Definition Language):
    * viene utilizzata dai progettisti per specificare lo schema concettuale del DB
    * può essere suddiviso ulteriormente in SLD (Storage Definition Langue) e VDL (View Definition Language) che servono ripettivametne per definire schemi interni e schiemi esterni.
* DML (Data Manipulation Language):
    * Utilizzati per interrogazioni e aggiornamenti del DB
    * I comandi dl DML possono essere integrati in linguaggi di Programmazione come C, Java, ...
    * Possono essere di 2 tipi: Alto Livello o Basso Livello (Procedurali)

Esistono varie intefaccie per un DB e le più comuni sono:

* Standalone Query Inerface
* Interfaccia per l'utilizzo di DML in linguagig di Programmazione
* Interfraccie UserFriendly

## Architettura Centralizzata

Esisono 2 possibili architetture:

* A 2 Livelli: il client comunica direttamente con il DBMS Server
* A 3 Livelli: le richieste del client vengono passati ad un Application Server intermedio che le analizza e fa da tramite con il DBMS Server. Questa è più sicura perchè non c'è un'interazione diretta tra Client e Server

## Classificazione DMBS

Ci sono vari criteri per classificacare i DMBS:

* In base al Modello dei Dati:
    * Tradizionale: Relazionali, Gerarchici, Reticolari
    * Emergenti: Oggetti, Relazionale da Oggetti
* In base a come è distribuito il DB:
    * Centralizzati
    * Distribuiti
* General Puroe vs Special Purpose

### Tradizionali

**Gerarchico**: 

* i dati vengono rappresentati come una struttura gerarchica ad albero
* Vantaggio: rispecchia la antura gerarchica di una molteplicità di domini
* Svantaggi: 
    * impone regole rigide
    * poco ottimizzabile per le query automatiche
    * i programmi dipendono dalle strutture
    * non si presta a rappresentare relazioni N:M
    * la definizione di relazioni genereiche richiede inserimento di duplciati

**Reticolare**:

* i dati vengono rappresetnati come record che sono collegati tra di loto tramite puntatori
* Vantaggi:
    * un record può avere più padri evitando quindi ridondanze
    * ongi nodo può essere il punto di partenza per raggiungere un determinato campo
    * permette di modellare relazioni N:M
* Svantaggi:
    * genera complesso reticolo di puntatori
    * poca ottimizzazione per le query automatiche

### Emergenti

**Oggetti**:

* definisce la base di dati intermini di oggetti, delle loro propiretà e delle loro operazini associate
* icorporano parti del paradigma ad oggetti (tipi astratti, incapsulamento, ereditarietà, ...)

## Modello Relazionale

Le differenze pricipali con i modelli ormai in disuso (reticolare e gerarchico) sono:

* modo in cui si rappresentano le associazioni tra i record
    * nei primi 2 si usano puntatori
    * nel Relazinale si usano valori
* La definizione del Modello Relazionale trae fondamento nella Teoria degli insiemi e della logica dei predicati al Primo Ordine

### Definizioni

![relazioni](./imgs/relazioni.png)

Un attributo rappresenta un campo della tabella<br>
Il Dominio rappresenta il tipo di elementi che saranno contenuti in un attributo.

![tupla](./imgs/tupla.png)

![schemarelazione](./imgs/schemarelazione.png)

![istanzarelazione](./imgs/istanzarelazione.png)

![basididati](./imgs/basididati.png)

_Esempio di Relazione:_
![tabellaesempio](./imgs/tabellaesempio.png)

### Vincoli nei modelli Relazionali

Ci sono vari tipi di vincoli nei modelli Relazionali:

* Vincoli Intrinseci: sono vincoli dettati dal tipo di dati che si vanno ad utilizzare (e.g. vincolo che una relazione non può avere tuple duplicate)

* Vincoli basati sullo Schema: vincoli che possono essere espressi direttamente sugli schemi del modello (tramite il DDL)

* Vincoli non esprimibili sullo schema: vincoli che vengono specificati tramite programmi applicativi

* Dipendenze Funzionali: usati per verificare la qualità dei DB Relazionali

#### Vincoli Basati sullo Schema

Questa categoria di vincoli può essere divisa in 2 sottocategorie:

* Intra Relazionali: coinvolge un unico schema relazionale
* Inter Relazionali: coinvolge più schemi relazionli

**Intrarelazionali**

Ci sono vari vincoli di questo tipo ma i più importanti sono:

* Vincoli di Tupla: sono vincoli che conivolgono valori della stessa tupla come:
    * Vincoli di Dominio: restrizioni sui tipi attribuibili ad un attributo
    * Vincoli su più valori della stessa tupla
    * Vincoli di valore non nullo
* Vincoli di Univocità: restrizioni che vietano a 2 tuple di una stessa istanza di coincidere su valori di un dato sottoinsieme di attributi.

![univocita](./imgs/univocita.png)

![superchiave](./imgs/superchiave.png)

Dato che le Chiavi Candidate possono ammettere valori nulli, è necessario scegliere una Chiave Candidata che svolga il ruolo di Chiave Primaria su cui non si ammettono valori nulli. Quest'ultima proprietà viene chiamata Vincolo di Integrità dell'entità

![integrita](./imgs/integrita.png)

![chiaveesterna](./imgs/chiaveesterna.png)

![chiaveesternamultipla](./imgs/chiaveesternamultipla.png)

![definizioneschemaistanza](./imgs/definizioneschemaistanza.png)

### Operazioni nel modello Relazionale :heart:

Le possibili operazioni che si possono effettuare in un DB relazionale sono:

* Inserimento: l'inserimento può violare tutti i tipi di vincoli. Quindi la maggior parte dei DBMS impedisce le operazioni di inserimento che violano tali regole

* Cancellazione: la cancellazione può portare alla violazione del vincolo di integrità referenziale, perciò i DBMS forniscono i seguenti approcci al problema:
    * rifiuto della cancellazione
    * propagazione della cancellazione: cancellare tutte le tuple che si riferiscono alla tupla che si sta eliminando
    * modifica dei valori referenti che causano la violazione ponendoli a Null o ad un valore di Default
* Modifica: la modifica può essere vista come un'operazioen di cancellazione e inserimento

## Algebra Relazionale

È un linguaggio per l'interrogazione dei DB Relazionali. Alcune carattirstiche:

* È costituito da un insieme di operazioni unari e binari su istanze di relazioni: ogni operatore ha come argomemnto 1 o 2 istanze di relazione e produce come output una istanza di relazione

* È un linguaggio procedurale: viene specificata la sequenza di operazioni neccessarie per ottenere il risultato atteso

Ci sono 2 tipi di Operatori:

* Operatori Insiemistici:
    * Unione
    * Intersezeione
    * Differenza
    * Prodotto Cartesiano
* Operatori Propriamente Relazionali:
    * Ridenominazione
    * Selezione
    * Proiezione
    * Concatenazione
    * Divisone

### Ridenominazione

![ridenominazione](./imgs/ridenominazione.png)

In breve, questo operatore permette di rinominare un singolo attributo, più attributi o una relazione, lasciando inalterati i valori.

Valgono le seguenti notazioni:

![ridenominazionenotazioni](./imgs/ridenominazionenotazioni.png)

### Unione

![relazioneunione](./imgs/relazioneunione.png)

![unione](./imgs/unione.png)

### Intersezione

![intersezione](./imgs/intersezione.png)

### Differenza

![differenza](./imgs/differenza.png)

> JUNGLE DIFF

### Prodotto Cartesiano

![prodottocart](./imgs/prodottocart.png)

### Selezione e Proiezione

![selezione1](./imgs/selezione1.png)

![selezione2](./imgs/selezione2.png)

![selezione3](./imgs/selezione3.png)

![proiezione1](./imgs/proiezione1.png)

### Concatenazione (Join)

![tetajoin](./imgs/tetajoin.png)

Viene effettuato un prodotto cartesiano tra r e s ed effettua una selezione in base alla condizione scelta F

**Equijoin**: le tuple vengono concatenate in base ad un attributo in comune e non come il teta-join che moltiplica tutto violentemente.

**Natural Join**: collassa gli attributi ridondanti dell'equijoin

![naturlajoin](./imgs/naturlajoin.png)

![naturlajoin](./imgs/naturlajoin2.png)

### Divisione

![divisione](./imgs/divisione.png)

![divisione2](./imgs/divisione2.png)

La divisione si usa per problemi del tipo "Trovare gli studenti che frequentano tutti i corsi".

### Raggruppamento e Funzini Aggregate

![reggruppamento](./imgs/reggruppamento.png)

![reggruppamento2](./imgs/reggruppamento2.png)

### Join Estesi

![joinsinistro](./imgs/joinsinistro.png)

![joindestro](./imgs/joindestro.png)

![joinfull](./imgs/joinfull.png)

### Null e algebra relazionale

> NULL POINTER EXCEPTION

![null1](./imgs/null1.png)

Le seguenti operazioni non cambiano : ![noncambia](./imgs/noncambia.png)

![of](./imgs/of.png)

![of2](./imgs/of2.png)

![joinnull](./imgs/joinnull.png)

## Calcolo Relazionale

![calcolo](./imgs/calcolo.png)

### Sintassi Calcolo Relazionale

![calcolo2](./imgs/calcolo2.png)

![atomo](./imgs/atomo.png)

### Formule e Semantica

![formulecalcolo1](./imgs/formulecalcolo1.png)

![formulecalcolo2](./imgs/formulecalcolo2.png)

### Variabili Libere o Legate

![vlibere](./imgs/vlibere.png)

![semantica](./imgs/semantica.png)

## Diagrammi ER (medici in prima linea)

![er](./imgs/er.png)

## Diagramma EER

![eer1](./imgs/eer1.png)

![eer2](./imgs/eer2.png)

![eer3](./imgs/eer3.png)

![eer4](./imgs/eer4.png)

![eer5](./imgs/eer5.png)

## Convertire ER in Schema Relazionale

[LEGGIMI SE HAI VOGLIA](http://www.dmi.unipg.it/raffaella.gentilini/BDlezioneProgLogicaChap7.pdf)

## Linee Guida per progettazione di DB

Le misure con cui si verifica la qualità di uno schema di relazione di un DB sono 4:

1. Semantica degli Attributi
2. Riduzione del numero di valori dirdondanti
3. Numero di valori NULLI nelle tuple
4. Presenza di Tuple Spurie
5. GAY

Per poter generare uno schema relazionale che sia il migliore possibile bisogna avere i seguenti accorgimenti:

* (**Semantica degli Attributi**) uno schema relazionale è migliore se è facile spiegare al semantica delle sue relazioni

* (**Riduzione dei valori ridondanti**) vanno ridotti i valori ridondanti nelle tuple per poter evitare sperchi di spazio e il generarsi di Anomalie di Aggiornamento (inserimento, cancellazione, aggiornamento)

* (**Riduzione dei valori nulli**) vanno ridotti i valori nulli all'interno delle relazioni per evitare spreco di spazio e per evitare problemi durante le operazioni di JOIN

* (**Tuple Spurie**) bisogna evitare che si vengano a generare tuple spurie tramite operazioni di JOIN

## Dipendenze Funzionali ANALI

### Obbiettivi

![dipfun1](./imgs/dipfun1.png)

Le dipendenze funzionali servono per:

* Determinare le chiavi candidate
* Esprimono Vincoli sull'ammissibilità delle istanze di relazione e sulla dipendeza tra attributi

![dipfun2](./imgs/dipfun2.png)

### Dipendenze Funzionali e Chiavi

![dipfun3](./imgs/dipfun3.png)

### Chiusura Indipendenze

Dato un insieme F di dipendenze funzionali, l'insieme delle dipendenze funzionali logicamente implicato da F è detto Chiusra di F e si indica con F+.<br>
Per determinare F+ si usano gli Assiomi di Armstrong

![armstrong](./imgs/armstrong.png)

#### Algoritmo per determinare F+

![algoarm](./imgs/algoarm.png)

#### Regole derivate

Per semplificare il calcolo di F+ si possono usare le seguenti regole derivate dagli Assimoi di Armstrong

![bello](./imgs/bello.png)

### Chiusura di un Insieme di Attributi

Dato un insieme di attributi a, la chiusura di a rispetto ad F (indicato con a+) è l'insieme di attributi determinati funzionalmente da a tramite le dipendenze in F.

![algo2](./imgs/algo2.png)

Questa operazione serve a:

![chiusuraserve](./imgs/chiusuraserve.png)

### Copertura Minimale

![minimal](./imgs/minimal.png)

Condizioni necessarie per Copertura Minimale:

![condizioni](./imgs/condizioni.png)

#### Come calcolare la Copertura Minimale

> Prendere 200g di uova, metterle a marinare per 72 ore nella salsa di SOYA e lacrime di ZINGARA, coprirle di cacca di YACC e lasciare riposare per 100 ANI.

Dato l'isnieme di dipenstze funzinoali F:

* Si imposti E = F
* Si sostituisca ogni dipendenza funzionale del tipo `X -> A1, A2, ..., An` con delle dipendenze funzionali del tipo `X -> A1, X -> A2, ..., X -> An`
* Si eliminino gli attributi ridondnati presenti nella parte Sinistra delle dipenze funzionali. Per ogni Dipendenza funionale `X -> A` in E e per ogni attributo B in X, se, rimuovendo la suddetta DF da E per poi aggiungerla nella forma `(X\B) -> A` (senza l'attributo B), E non cambia, allora B è un attributo Ridondante e la DF `X -> A` verrà sostituita in E con `(X\B) -> A`.
* Si eliminino le dipendenze funzionali ridonandti

<!-- LE TRASFORMAZIONI NON VANNO VIA -->

## Forme Normali

Esistono le seguenti forme normali che servono per Normalizzare uno schema Relazionale R:

* 1 NF
* 2 NF
* 3 NF
* BCNF

![chomksy](./imgs/chomksy.png)

Normalizzare vuol dire:

* Minimizzare la Ridondanza
* Avere una decomposizione Losslessssss-Join
* Conservare le dipendenze, ossia `(F1 U F2 U ... Fn)+ = F+`
    * violando la conservazione delle dipendenze è obbligatorio effettuare operazinoi di Join esplicite

### Verificare la Cosnervazione delle DF

Per verificare la se le dipendenze sono preservate bisogna controllare se tutte le dipendenze di partenza sono preservate in almeno una delle Ri generate dalla decomposizione

È un algoritmo dal costo polinomiale a differenza della Computazone di F+ richiede tempo esponenziale (Pinotti Infelice)

### BCNF

#### Definizione

Uno schema di Rleazione `R(x)` è in BCNF rispetto ad un insieme F di DF, se per ogni DF di F+, nella forma `a -> b`, `a` è una superchiave

#### Algoritmo per decomposizione BCNF

Data uno schema R(x), per ogni DF nella forma `a -> b` bisogna controllare se il termine a Sinistra è una superchiave (se viola la BCNF) e se non lo è, bisogna decomporre R in `R1 = (R\b)` e `R2 = (a, b)`.

Con la BCNF non è sempre possibile mantenere la Conservazione delle Dipendenze come in questo esempio:

`R(J, K, L), F={JK -> L, L -> K}`

### 3NF

Per ovviare al fatto che non sempre si possono conservare le dipendenze si introduce la 3NF, la quale è una forma più debolede della BCNF (perchè ammette ridondanze) che permette di mantenere le DF.
Esiste sempre una decomposizione in 3NF che mantiene le dipendenze ed è senza perdite.

#### Definizione

Uno schema di Rleazione `R(x)` è in 3NF rispetto ad un insieme F di DF, se per ogni DF di F+, nella forma `a -> b`, sono rispettate almeno una di queste regole:

* `a -> b` è banale
* a è una Superchiave
* ogni attributo A di `b\a` appertiene ad una chiave andidata


#### Algoritmo di Decomposizione

Sia G una copertura canonica in F.<br>
Per ogni parte sinistra X di una DF (`X -> A`) in G si crei un nuovo Schema `D(X, A1, A2, ..., An` dove `X -> A1, X -> A2, ..., X - > An`. X sarà la chiave dello schema.<br>
Se nessuno degli schemi D contiene una chiave di R, si crei uno nuovo schema D contenente attributi che formano una chiave di R.<br>
Si eliminino le relazioni ridondanti.
