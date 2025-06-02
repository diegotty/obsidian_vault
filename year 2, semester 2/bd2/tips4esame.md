---
related to: 
created: 2025-03-02T17:41
updated: 2025-06-02T22:40
completed: false
---
>[!info] ngl
>- durata > inizio e fine (evita vincoli su date !!!!!)

- vincoli esterni: è necessario specificare TUTTI i vincoli esterni necessari (in particolare, i vincoli banali: date, )
- le operazioni sono necessarie quando vanno a sostituire un attributo che va calcolato
	- se invece servono a calcolare qualcosa che non sarebbe un dato, sono use-case

## progettazione
### ristrutturazione
- eliminazione di attributi multivalore (classe per attributo (con $\{ id \}$ e associazione con molteplicità)
- scelta dei tipi di dato (scrivere tipi non base in SQL ?)
	- tipi concettuali base
	- tipi base specializzati: domain
	- tipi enumerativi: type as enum
	- tipi composti: create type (se campi `NOT NULL`, bisogna creare domini ausiliari (nei domini è consentito usare `NOT NULL`))
- generalizzazioni (attenzione ai vincoli esterni !!) (la decisione deriva in parte dalla natura delle query !)
	- classi:
		- *fusione* **di sottoclassi in superclassi** (se mi interessa fare query specifiche sulla superclasse, e non sulle sottoclassi)
		- *divisione* **delle superclassi in classi disgiunte** (deve esserci `{disjoint,complete}`, utile se dobbiamo fare query specifiche per entrambe le sottoclassi, e non ci interessano query sulla superclasse)
		- *sostituzione* **di relazioni is-a con associazioni** (se mi interessa rappresentare superclasse e sottoclassi (**strettamente**) in modo separato)
	- associazioni
		- *fusione* **di sotto-associazioni nelle loro super-associazioni**
- identificatori: se ci sono già $\{id\}$, controlliamo se uno di loro basta. altrimenti usiamo un **identificatore artificiale**
- vincoli esterni e use-case (in FOL)

>[!warning] operazioni non sono da ristrutturare
### traduzione diretta
- associazioni:
	- molteplicità:
	- 0..* - 0..* : chiave è composta dalle foreign key
	- 0..* - 0..1 : chiave è composta da foreign key del ruolo 0..1
	- 0..1 - 0..1 : chiave è composta da una delle due foreign key (è uguale)
	- 1..\* - 0..\*: **vincolo di inclusione**: ogni chiave di ruolo 1../* deve apparire in tabella associazione 
	- 1..\* - 1..\*: 2 **vincoli di inclusione**
	- 1..1 - 0..* : **vincolo di inclusione** che può esser implementato con vincolo di foreign key da parte del ruolo 1..1 (quindi doppia foreign key)
	 - $\{ id1 \}$ su 1..1 e attributo : **accorpamento** nel verso della tabella che viene riconosciuta con l’associazione. campo `chiave_tabella_opposta`, foreign key
	 - $\{ id \}$ unicamente su associazione (ristrutturazione per sostituzione): attributo `chiave_tabella_opposta` che è anche chiave primaria (e foreign key)
>[!warning] possiamo accorpare ogni volta che abbiamo molteplicità `0..1` o `1..1`

>[!warning] UNIQUE !!!

### vincoli
i vincoli esterni devono diventare:
- vincoli di ennupla (vincoli esterni che sono nella scope di un’unica tabella)
- trigger (vincoli esterni che necessitano il check su altre tabelle oltre alla propria)

>[!info] domande
>mettere vincoli dominio e usare domini è la stessa cosa giusto ….