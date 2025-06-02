---
related to: 
created: 2025-03-02T17:41
updated: 2025-06-02T16:33
completed: false
---
- vincoli esterni: è necessario specificare TUTTI i vincoli esterni necessari (in particolare, i vincoli banali: date, )
- le operazioni sono necessarie quando vanno a sostituire un attributo che va calcolato
	- se invece servono a calcolare qualcosa che non sarebbe un dato, sono use-case

ristrutturazione:
- eliminazione di attributi multivalore (classe per attributo (con $\{ id \}$ e associazione con molteplicità)
- scelta dei tipi di dato (scrivere tipi non base in SQL ?)
	- tipi concettuali base
	- tipi base specializzati: domain
	- tipi enumerativi: type as enum
	- tipi composti: create type (se campi `NOT NULL`, bisogna creare domini ausiliari (nei domini è consentito usare `NOT NULL`))
- generalizzazioni (attenzione ai vincoli esterni !!) (la decisione deriva in parte dalla natura delle query !)
	- classi:
		- *fusione* **di sottoclassi in superclassi** (se mi interessa fare query specifiche sulla superclasse, e non sulle sottoclassi)
		- *divisione* **delle superclassi in classi disgiunte** (deve esserci `disjoint,complete`, utile se dobbiamo fare query specifiche per entrambe le sottoclassi, e non ci interessano query sulla superclasse)
		- *sostituzione* **di relazioni is-a con associazioni** (se mi interessa rappresentare superclasse e sottoclassi (**strettamente**) in modo separato)
	- associazioni
		- *fusione* **di sotto-associazioni nelle loro super-associazioni**
- identificatori: se ci sono già $\{id\}$, controlliamo se uno di loro basta. altrimenti usiamo un **identificatore artificiale**
- vincoli esterni e use-case (in FOL)

>[!warning] operazioni non sono da ristrutturare