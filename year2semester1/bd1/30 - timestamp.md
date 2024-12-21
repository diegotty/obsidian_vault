---
created: 2024-12-21
related to: 
updated: 2024-12-21T09:26
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
>[!important] quindi uno schedule è serializzabile se per ciascun item acceduto da più di una transazione, l’ordine con cui le transazioni accedono all’item è quello imposto dai timestamp !!

>[!example] esempio 1
consideriamo le seguenti transazioni:
![[Pasted image 20241221091242.png]]
con $TS(T_{1}) =110\,\,\,\,\,\, TS(T_{2})=100$
in questo caso, uno schedule è serializzabile se è **equivalente** allo schedule seriale $T_{2}T_{1}$(in cui l’item $X$ è acceduto prima da $T_{2}$ e poi da $T_{1}$, seguendo l’ordine dei timestamp)
>
>consideriamo il seguente schedule:
![[Pasted image 20241221091514.png|200]]
lo schedule non è serializzabile, in quanto $T_{1}$ legge $X$ prima che $T_{2}$, l’abbia scritto (quindi per accesso si intende `read + write`)

>[!example] esempio 2
consideriamo le seguenti transazioni:
![[Pasted image 20241221091808.png]]
con $TS(T_{1}) =110\,\,\,\,\,\, TS(T_{2})=100$
>
>il cui schedule seriale $T_{1}T_{2}$ è:
![[Pasted image 20241221091838.png|200]]
>
>consideriamo il seguente schedule:
>![[Pasted image 20241221092118.png|200]]
lo schedule è serializzabile solo se **non viene eseguita** la scrittura di $X$ da parte di $T_2$, in quanto $T_{2}$ dovrebbe accedere prima a $X$ (anche se solo con `write`), invece in questo schedule accede prima $T_{1}$. quindi $T_{2}$ non dovrebbe accedere a $X$ per rendere lo schedule equivalente a $T_{1}T_{2}$
## read timestamp, write timestamp
a ciascun item $X$ vengono associati due timestamp:
- **read timestamp** di $X$ (`read_TS(X)`), cioè il più grande fra tutti i tem
- **write timestamp** di $X$ (`write_TS(X)`)