---
created: 2024-12-21
related to: 
updated: 2024-12-21T09:10
---
# timestamp
il timestamp identifica una transazione, ed è assegnato alla transazione dallo scheduler quando la transazione ha inizio
esso può essere:
- il valore di un contatore
- l’ora di inizio della transazione
quindi: il timestamp della transazione $T_{1}$ è minore del timestamp della transazione $T_{2}$, la transazione $T_{1}$  è iniziata prima della transazione $T_{2}$
- quindi, se la transazioni fossero eseguite in modo seriale, verrebbe eseguita $T_{1}$ e poi $T_{2}$
## serializzabilità
uno schedule è serializzabile se è equivalente allo schedule seriale in cui le transazioni compaiono ordinate in base al loro timestamp