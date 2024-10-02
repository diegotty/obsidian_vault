linguaggio usato per interrogare una base di dati relazionale, consiste di un insieme di operatori che possono essere applicati ad una o due istande di relazione, e forniscono come risultato una nuova istanza di relazione
viene definito linguaggio procedurale, in quanto l’interrogazione consiste in un’espressione in cui compaiono operatori dell’algebra e istanze di relazioni, in una sequenza che prescrive in maniera puntuale l’ordine delle operazioni
## proiezione
taglio verticale su una relazione(selezionare solo alcuni attributi)
- si denota con il simbolo $\pi$
, selezionando gli attributi della relazione (in questo caso r)
$$\pi_{\text{A1,A2,...Ak}}(r)$$
>![warning] si seguono le regole insiemistiche ! nella relazione risultato NON CI SONO DUPLICATI. per avere anche i doppioni bisogna includere una chiave nella proiezione
## selezione
selezione di alcune righe che soddisfano una condizione
- si denota con il simbolo $$\sigma$$
selezionando la condizione C con cui “filtare” le tuple della relazione
$$\sigma_{\text{C}}(r)$$
la condizione e’ un’espressione booleana composta, con:
- operatori AND, OR, e NOT
- termini semplici: $A\theta B$, $A\theta  'a'$, dove $\theta$ e’ un operatore di confronto (<,>,=,≤, ≥), A e B sono attributi _con lo stesso dominio_ e `a` e’ un elemento di dom(A)
##