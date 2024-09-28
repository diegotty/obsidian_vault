- proposto per favorire l’indipendenza dei dati
- basato sulla nozione matematica di relazione. le relazioni si traducono in maniera naturale in tabelle
## relazioni
la parola relazione puo’ essere usata per indicare concetti diversi:
- relazione matematica: come nella teoria degli insiemi
- relazione, secondo il modello relazionale dei dati (?)
- relazione(associazione) nel modello concettuale Entity-Relationship, che rappresenta un tipo di collegamento concettuale tra Entita’ diverse.
## dominio
tutti i valori (possibilmente infiniti) di un insieme (che poi detteranno i valori assumibili da un attributo)
- una relazione matematica e’ un qualsiasi sottoinsieme del prodotto cartesiano di uno o piu’ domini !
- una tupla e’ un elemento del prodotto cartesiamo dei domini dello schema (e di conseguenza non ci possono essere duplicati tra le tuple). ovviamente non tutti i valori del prodotto cartesiano dei domini dello schema hanno un senso compiuto.
- le tuple non hanno un ordinamento
di solito si usano i domini predefiniti: String, Integer, Real, etc….
### NULL
inoltre, tutti i campi (se non specificato altrimenti o se esistono altri vincoli impliciti (chiavi)) possono avere il valore NULL, che rappresenta mancanza di informazione o il fatto che l’informazione non sia applicabile per quella tupla
>[!] e’ meglio usare null per rappresentare la mancanza di informazione, al posto che valori del dominio un
### grado e cardinalita’ di una relazione
- grado → numero dei domini (attributi)
- cardinalita’ → numero di tuple
## attributi
un attributo e’ definito da un nome A(univoco in una tabella) e dal suo `dominio` , che viene indicato con dom(A)
ennupla → funzione che, dato un insieme di attributi R, associa ad ogni attributo A in R un elemento di dom(A)
>[!info] 
>l’insieme di attributi di una relazione e’ detto schema, e data una relazione R e i suoi attributi A1, A2, ….., Ak, e’ indicato da R(A1, A2, …., Ak)
>- lo schema descrive quindi la struttura della relazione (aspetto intensionale), mentre l’istanza della relazione e’ l’aspetto estensionale

## dati fra relazioni diverse
nel modello relazionale i riferimenti fra dati in relazioni diverse sono rappresentati per mezzo di valori dei domini che compaiono nelle ennuple
