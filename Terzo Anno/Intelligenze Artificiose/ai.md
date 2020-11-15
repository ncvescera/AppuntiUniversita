# Intelligenze Artificiose

## Agenti Intelligenti

> L'intelligente lo è chi l'intelligente lo fa

**Definizione:** <br>
Un agente è qualsiasi cosa che può percepire l'ambiente circostante tramite sensori e agire su di esso tramite attuatori.

![Agente](./imgs/agente.png)

Le azioni dell'agente sono influenzate dal tipo di ambiente in cui si trova.<br>
Gli ambienti vengono classificati in base ai seguenti criteri:

* **Osservabilità**:
    
    * **Fully Observable**: i sensori dell'agente gli danno accesso allo stato completo dell'ambiente in qualisasi momento.

    * **Partially Observable**: l'agente ha una conoscenza parziale dell'ambiente che lo circonda (può essere causato dalla limitatezza dei sensori o dalla natura stessa dell'ambiente)

* **Single Anget Vs Multi Agent**: possibilità di avere un singolo agente o molteplici che possono essere **Competitivi** o **Cooperativi**

* **Determinabilità**:

    * **Deterministico**: lo stato successivo dell'ambiente è determinato completamente dallo stato attuale e dalle azioni effettuate dall'agente

    * **NON Deterministico**: tutto ciò che non è deterministo è dunque NON Deterministico (`best effort !`)

* **Episodico Vs Sequenziale**:

    * **Episodico**: gli stati futuri non dipendono dalle azioni svolte in precedenza dall'agente (come il controllore di difetti in una linea di assemblaggio)

    * **Sequenziale**: il contrario di Episodico (`best effort !`)

* **Dinamicità**:

    * **Statico**: l'ambiente cambia solamente quando l'agente effettua delle azioni

    * **Dinamico**: l'ambiente può cambiare mentre l'agente sta pensando

* **Discreto Vs Continuo**: dipende dalla maniera in cui il tempo e lo stato dell'ambiente sono gestiti dall'agente, per esempio: una partita di scacchi è discreta mentre un gioco di strategia no.

* **Conoscibilità**: la conoscenza dell'agente rispetto alle leggi dell'ambiente in cui si trova.<br>
_E.g._: In un ambiente conosciuto data una serie di azioni si conosce il risultato.

## Algoritmi di Ricerca

Dato un problema sconosciuto l'agente può operare in 2 modi:

* Compiere azioni random sperando di raggiungere la soluzione

* Seguire il seguente processo di problem solving:

    1. Goal Formulation
    2. Problem Formulation
    3. Search
    4. Execution

Durante le fase di Execution gli agenti possono utilizzare 2 modelli di systemi:

* **Open Loop**: gli agenti hanno la certezza che durante un'azione lo stato dell'ambiente non venga alterato

* **Closed Loop**: l'opposto di Open Loop (`best effort !`)

### Definizioni di Algoritmi di Ricerca

* **Problema**: l'insieme degli stati possibili nei quali l'ambiente può esistere, viene anche chiamato **Spazio degli Stati**

* **Stato iniziale**: lo stato in cui l'agente inizia

* **Stato Goal/Finale**: lo stato che l'agente vuole raggiungere

* **Azioni**: dato uno stato `s` la funzione `action(s)` ritorna un insieme finito di azione che possono essere eseguite in s e sono dette **applicabili in s**.<br>
`ACTION(Arad) = {ToSibiu, ToTimisoara, ToZerind}`

* **Modello di Transizione**: ritorna lo stato che risulta dall'applicazione di una determinata azione `a` in uno stato `s`.<br>
`RESULT(Arad, ToZerind) = Zerind`

* **Funzione Costo Azione**: ritorna il costo numerico richiesto per applicare un'azione `a` in uno stato `s` per raggiungere lo stato `s'`.

* **Nodi di Frontiera**: i nodi visti ma non espansi.

* **Cammino**: la conseguenza di insieme di azioni.

* **Cammino Ridondante**: è un cammino che porta allo stesso nodo ma con costo maggiore. Un ciclo è un particolare tipo di cammino ridondante.

* **Soluzione**: il cammino che va dallo stato iniziale ad uno stato goal/finale.<br>
Una soluzione si dice **ottima** se corrisponde al percorso di costo minore tra tutte le soluzioni.

### Rappresentazione di un problema di ricerca

Un problema di ricerca può essere rappresentato in 2 modi:

1. Lo State Space Graph (il grafo dello spazzio degli stati)
2. L'Albero di Ricerca


Le principali differenze tra i 2 sono che:

|State Space Graph|Albero di Ricerca|
|-----------------|-----------------|
|Ad ogni nodo corrisponde uno stato e ogni arco corrisponde ad un'azione che può esser eseguita in quello stato.|Ci possono essere più nodi uguali, però dato un nodo, il cammino che lo riporta alla radice è unico.|
|_Ti fa vedere tutto il problema_| _Qui vedi più chiaramente il cammino_.|
|![Romania ia ia oh](./imgs/romania.png)|![albero di ricerca](./imgs/searchtree.png)|


L'albero di ricerca riportato sopra presenta un ciclo tra `Arad` e `Sibiu`. Poichè i cilci possono essere percorsi infinite volte, l'albero di ricerca **completo** può avere infiniti nodi !


Questa mappa verrà presa in considerazione per i successivi esempi:
![Romania ia ia oh](./imgs/romania.png)


### Il Problema dei Cammini Ridondanti

I cammini ridondanti possono andare a complicare di molto alcuni problemi di ricerca e quindi vanno evitati.<br>
Ci sono 3 diversi modi per farlo:

1. **Memorizzare i nodi visitati**. Questo metodo è particolarmente utile se si hanno tanti cammini ridondanti e se abbiamo abbastanza spazio in memoria per rappresentare la tabella degli stati raggiunti.

2. **Metodo Milani**: se non c'è bisogno di farlo non lo fare ! Eseisto alcuni casi in cui non è necessario tenere traccia degli stati già visti perchè la struttura del problema impedisce a determinate azione di essere effettuate.<br>
_E.g._ una linea di assemblaggio.

3. **Parent Link**: si controlla solo la presenza di cicli e non di cammini ridondanti. Ogni nodo ha un link al proprio nodo padre e si risale all'indietro questa "catena" per vedere se vi è un ciclo.<br>
Alcune implementazini risalgono tutta la catena, impiegano molto tempo ma tolgono tutti i cicli, altre solo una piccola parte (3 o 4 salti all'indietro), impiegano un tempo costante ma riescono a togliere solo piccoli cicli.


### Graph Search vs Tree-like Search

Possiamo fare una distinzione degli algoritmi in base alla necessità di controllare la presenza di cammini ridondanti:


|Graph Search|Tree-like Search|
|------------|----------------|
|Questi algoritmi controllano la presenza di cammini ridondanti|**NON** controllano la presenza di cammini ridondanti.
|Best First Search|Assembly Problem|


### Complessità degli algoritmi di ricerca

Per decidere quale algoritmo di ricerca utilizzare è necessario guardare alle loro diverse caratteristiche di complessità.<br>
Le 4 che vengono prese in considerazione sono:

* **Completezza**: la capacità di un algoritmo di garantire il ritrovamento della soluzione, se ce n'è una, o di riportare il fallimento se non ne viene trovata nessuna.
* **Ottimalità di costo**: la capacità di trovare il cammino di costo minimo
* **Complessità di tempo**: il tempo richiesto per torvare una soluzione. Può essere misurato in secondi o in numero di stati analizzati e di azioni compiute.
* **Complessità in spazio**: la memoria utilizzata pe l'esecuzione dell'algoritmo.


### Spazzio degli stati infinito

Quando lo spazio degli stati è finito non ci sono grandi problemi per la ricerca di una soluzione. Qunando invece si tratta di uno spazio degli stati infinito, la ricerca deve essere fatta **sistematicamente** per evitare di applicare sempre la stessa azione e non tornare mai indietro per controllare stati vicini allo stato iniziale.


### State Space Graph Implicito ed Esplicito

In un State Space Graph **Esplicito** generalmente il calcolo della complessità non è difficoltoso poichè basta rappresentarlo con un search tree e la complessità risulterà: `|V| + |E|` con:

* `|V|` numero di nodi
* `|E|` numero degli archi

In alcuni problemi di IA, tuttavia, il grafo può essere rappresentato in maniera **Implicita**.<br>
In questi casi la complessità può essere misurata in funzione di 3 fattori:

* `d` (**_depth_**): il numero di azioni in una soluzione ottimale
* `m`: massimo numero di azioni in un cammino qualsiasi
* `b` (**_brancing factor_**): il numero di successori ad un nodo che deve essere preso in considerazione


### Best First Seach

Uno degli algoritmi di ricerca più semplici è il **Best First Search**, esso basa la scelta del nodo su una funzione di valutazione, scelta arbitrariamente, chiamata `f(n)`.<br>
Ad ogni iterazione viene scelto il nodo di frontiera con `f(n)` minimo e:

* se è lo stato Goal ritorna il corrispondente cammino
* altrimenti applica la funzione `EXPAND()` per generare altri nodi figli e li aggiunge alla frontiera se non sono mai stati raggiunti o sono riaggiunti se ora possono essere raggiunti con un cammino di costo inferiore a quello precedente.<br>
Questo algoritmo ritorna un avviso di fallimento o un cammino che rappresenta la soluzione.

```javascript
//problem contiene:
//  Insieme degli Stati
//  Stato Iniziale
//  Stato Goal/Finale
//f:
//  funzione di valutazione costo
function BestFirstSerach(problem, f) {
    // stato iniziale del problema
    node = problem.getInitalNode()

    // priority queue basata su F e inizia con node
    frontier = []
    frontier.add(node)

    // tabella di LookUp (tipo una HashMap) inizializzata 
    // con key=NodoIniziale value=CostoDelNodo
    reached = LookUpTable()
    reached[node.STATE] = node

    // fin quando non ci sono più elementi in frontiera
    while (frontier is not Empty) {
        // prende l'elemento di costo minore
        node = frontier.pop()

        if isGoal(node.STATE)
            return node
        
        foreach (child in expand(problem, node)) {
            s = child.STATE

            // aggiunge il nodo alla frontiera se:
            //  non è mai stato visitato
            //  è raggiungibile con costo minore
            if (s not in reached) or (child.PATH_COST < reached[s].PATH_COST) {
                reached[s] = child
                frontier.add(child)
            }
        }
    }

    return failure
}

//problem contiene:
//  Insieme degli Stati
//  Stato Iniziale
//  Stato Goal/Finale
//node:
//  il nodo da espandere
function expand(problem, node) {
    s = node.STATE

    // trova tutti i nodi raggiungibili da node e ne calcola i costi
    foreach (action in problem.actions(s)) {
        s1 = problem.result(s, action)
        cost = node.PATH_COST + problem.actionCost(s, action, s1)
        yield Node(STATE=s1, PARENT=node, ACTION=action, PATH_COST=cost)
    }
}
```

In questo algoritmo vengono scartati i **Cammini Ridondanti**.


### Agente Informato vs Non Informato

Un Agente **non informato** non sa se scegliendo una soluzione andrà ad avvicinarsi al Goal, quindi date 2 possibili azioni,  mentre un agente informato si.


### Ricerca Non Informata

A questa famiglia di ricerca appartengono:

* **Breadth First Search**
* **Best First Search** (Dijisktra) <!-- ricordarsi che Jhonson fa un pompino a Dijkstra -->
* **Depth First Search**
* **Depth Limited Search**
* **Iterative Deeeeeeepening Search**

#### Breadth Firs Search

<!-- Aggiungere BREACKFAST -->

Quando le azioni hanno tutte quante lo stesso costo può essere una buona idea utilizzare la Breadth First Search. Essa è molto simile alla Best First Search, ma come funzione di valutazione `f(n)` considera non il costo delle azioni, ma la **profondità** del nodo, ovvero il numero di azioni richieste per raggiungerlo.

Nell'implementazione è possibile migliorarlo alterando alcuni aspetti della Best First Search:

* La coda **frontier** può essere impelemntata come una coda FIFO dato che darà una coda che rispetta già l'ordine di visita per la Bereadth First Search (i più vecchi vengono visitati prima)
* La tabella **reached** può essere impostata con gli stati piuttosto che con una mappatura stati-nodi
* E' possibile effettuare un **early goal test** poichè, una volta trovato un cammino, saremo sicuri che non ci saranno altri cammini migliori per raggiungere quel nodo

Questo algoritmo è **Completo** e **Ottimo**.<br>
Ha una complessità in **tempo** e **spazio** equvalenti che sono: ![O(b^d)](./imgs/o_b_d.gif)

Quando lo spazio delle soluzioni è molto ampio e si ha un brancing factor elevato, non è possibile applicare algoritmi di ricerca non informati poichè si va in contro a limitazioni fisiche di memoria e, anche ipotizzando una memoria infinita, si raggiungono tempi di esecuzione impraticabile (per uno stato goal con d = 14 e b = 10 il tempo di esecuzione richiesto sarebbe di 3.5 anni)

```javascript
function breadthFirstSearch(problem) {
    // stato iniziale del problema
    node = problem.getInitalNode()

    // goal test per terminare immediatamente se in stato finale
    if isGoal(node.STATE)
        return node
    
    // coda FIFO inizializzata con il nodo node
    frontier = []
    frontier.append(node)

    // lista di stati visitati
    reached = []
    reached.append(node.STATE)

    while (frontier is not Empty) {
        node = frontier.pop()

        // espande i figli del nodo estratto dalla fifo
        foreach (child in expand(problem, node)) {
            s = child.STATE

            // early goal test
            if isGoal(s)
                return child
            
            // aggiunge il figlio alla frontiera
            if (s not in reached) {
                reached.append(s)
                frontier.append(child)
            }
        }
    }

    return failure
}
```

#### Algoritmo di Dijstra o Uniform Cost Search

Questo algoritmo espande, a differenza della breadth first serach che espande i nodi in base alla profondità, i nodi in base al costo del cammino totale: espande prima i commini con lo stesso costo.<br>
Non si espande nodo in nodo ma cammino in cammino.

```javascript
function uniformCostSearch(problem) {
    //PATH_COST è una funzione che si basa sul costo totale del percorso
    return bestFirstSearch(problem, PATH_COST)
}
```
