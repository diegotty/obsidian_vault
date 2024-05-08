fasei dell’istruzione:
- **fetch** (**IF**): carica l’istruzione dalla memoria
- **instruction decode** (**ID**): la CU decodifica l’istruzione e vengono letti gli argomenti dai registri
- **execution** (**EXE**): l’ALU fa il calcolo necessario (tipo R o accesso alla memoria o branch)
- **memory access** (**MEM**): viene letta/scritta la memoria (lw e sw)
- **write back** (**WB**): il risultato dell’ALU o quello letto dalla memoria viene messo nel registro dest.

in una CPU a un ciclo di clock, in ogni momento è attiva solo un’unità funzionale.
per rendere il più efficiente possibile la nostra architettura, possiamo scomporre l’esecuzione di un’istruzione in una catena di montaggio (**pipeline**) dove ogni fase svolge il compito ad essa assegnatogli per poi passare il risultato alla fase successiva, procedendo ad elaborare già la stessa fase dell’istruzione successiva.

![[Pasted image 20240508134210.png]]

in questo modo:
- posso scegliere un periodo di clock molto più corto !! (da 800 ps a 200ps)
- riducendo il periodo di clock ad un quarto, quadruplico la velocità
## lettura e scrittura dal blocco Registri
si può notare come quando vengono eseguite 5 istruzioni contempora