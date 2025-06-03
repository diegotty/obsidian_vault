---
created: 2025-05-06T13:13
updated: 2025-06-03T15:26
---
>[!index]
>- [obiettivi](#obiettivi)
>- [specifica dei requisiti](#specifica%20dei%20requisiti)
>- [raffinamento dei requisiti](#raffinamento%20dei%20requisiti)
>- [diagramma UML](#diagramma%20UML)
>- [specifica dei tipi di dato](#specifica%20dei%20tipi%20di%20dato)
>- [specifica di classe](#specifica%20di%20classe)
>- [diagramma UML use-case](#diagramma%20UML%20use-case)
>- [specifica degli use-case](#specifica%20degli%20use-case)
## obiettivi
## specifica dei requisiti
## raffinamento dei requisiti
## diagramma UML
## specifica dei tipi di dato
## specifica di classe
### classe Letto
[V.letto.no_due_ricoveri_simult]
due periodi si sovrappongono se
EXISTS t DataOra(t) and comprende(t, periodo1) and comprende(t, periodo2)

due pazienti non può essere parte di 2 ricoveri simultanei (ma 2 prestazioni si )


comprende viene implementata in Ricovero e deve essere reimplementata in RicoveroTerminato in quanto il suo significato è diverso (esempio di specializzazione delle operazioni)
usando la classe Letti non dobbiamo gestire vincoli strani

possiamo aggiungere campi che sono utili (tipo cf per persona in quickhospital) / per modellare meglio la realtà


paradigma dell’ago nel pagliaio:
un medico ha tante specializzaizoni, ma una di esse è primaria (è speciale)
## diagramma UML use-case


## specifica degli use-case