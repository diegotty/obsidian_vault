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
- si denota con il simbolo $\sigma$
selezionando la condizione C con cui “filtare” le tuple della relazione
$$\sigma_{\text{C}}(r)$$
la condizione e’ un’espressione booleana composta, con:
- operatori AND, OR, e NOT
- termini semplici: $A\theta B$, $A\theta  'a'$, dove $\theta$ e’ un operatore di confronto (<,>,=,≤, ≥), A e B sono attributi _con lo stesso dominio_ e `a` e’ un elemento di dom(A)
## unione
consente di costruire una relazione contenente tutte le tuple che appartengono al meno a uno dei 2 operandi.
- si denota con il simbolo $\cup$ 
>[!info] union-compatibilita’
>l’unione, come altre operazioni, puo’ essere applicata solo a operandi union-compatibili, cioe’:
>- che hanno lo stesso numero di attributi(possiamo usare la proiezione per eliminare gli attributi che ci impedirebbero di fare un unione)
>- i cui attributi corrispondenti, in ordine(l’ordine degli attributi influenza l’union-compatibilita’), sono definiti sullo stesso dominio
la union-compatibilita’, quindi, si guarda sugli schemi delle relazioni

non e’ necessario che gli attributi abbiano lo stesso nome, ma ovviamente il risultato ha senso solo se hanno un significato omogeneo !! (e’ quasi un requisito, insieme alla union-compatibilita)
- Eta e CFU possono essere definiti sullo stesso dominio, ma non averebbe senso unire le due relazioni che hanno questi attributi in colonne corrispondenti
il risultato dell’unione non ha duplicati (anche in sql!)
## differenza
consente di costruire una relazione contenente tutte le tuple che appartengono al primo operando e NON apparengono al secondo operando
- si applica a operandi union-compatibili
- si denota con il simbolo $-$  $$r_{1} - r_{2}$$
la differenza non e’ commutativa come l’unione! l’ordine degli operandi cambia il risultato della query.
## intersezione
consente di costruire una relazione contente tutte le tuple che appartengono ad entrambi gli operandi
- si denota con il simbolo $\cap$
- si applica a operandi union-compatibili
$$ 