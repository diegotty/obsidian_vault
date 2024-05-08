se la CU genera segnali errati, bisogna individuare: 
- quale combinazione di segnali venga generata
- quali istruzioni vengano influenzate dalle nuove combinazioni e cosa facciano
una volta individuate le nuove funzionalità, si può scrivere un breve programma che evidenzia se la CPU sia malfunzionante o meno
## regWrite ← Branch
si ha il dubbio che, per difetto di progettazione della CU:
- il segnale RegWrite sia determinato dal segnale Branch(RegWrite == Branch)
si assume che:
- MemToReg = 1 solo per lw ed altrimenti valga 0
- RegDest = 1 solo per le istruzioni di tipo R e altrimenti valga 0
le istruzioni affet