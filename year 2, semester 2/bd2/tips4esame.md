---
related to: 
created: 2025-03-02T17:41
updated: 2025-06-02T16:25
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
	- tipi composti: create type (se campi `NOT NULL` , bisogna creare domini ausiliari (nei domini è consentito usare `NOT NULL`))