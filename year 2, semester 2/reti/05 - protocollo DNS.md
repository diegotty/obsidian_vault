---
related to: 
created: 2025-03-02T17:41
updated: 2025-03-17T23:15
completed: false
---

# indirizzo IP
gli host Internt hanno i propri **hostname** (`www.google.com`, `w3.uniroma1.it`, ….), ma i nomi, anche se facili da ricoradare, forniscono poca informazione sulla collocazione degli host all’interno dell’Internet(`.it` ci dice che l’host si trova probabilmente in Italia, ma non dove)
un **indirizzo IP** ci permette di identificare un host in una rete, e anche si sapere dove si trova tale host !
- consiste di 4 byte, ed è costituito da una stringa in cui ogni punto separa dei byte espressi con un numero decimale da 0 a 255
- presenta una struttura gerarchica
# DNS
ora che abbiamo un hostname ed un indirizzo IP, dobbiamo trovare un sistema per matcharli, memorizzarli, e renderli accessibili
il **DNS**(domain name system) rende ciò possibile:
- **memorizazione** → database distribuito, implementato in una gerarchia di server DNS
- **accessibilità** → protocollo a livello di applicazione che consente agli host di interrogare il database distribuito per **risolvere**(tradurre) i nomi(in indirizzi)
>[!info] il DNS viene quindi usato dagli altri protocolli di livello applicazione per tradurre hostname in indirizzi IP
>utilizza il trasporto UDP e indirizza la porta 53 ! wooooo (…)

dobbiamo matchare il nome (comodo per noi) e indirizzo ip

come recuperiamo IP da nome ? chi mantiene il mapping ? il DNS

questa infomazione deve essere accessibile in tempi veloci e distribuirla (tanti ip ….)


distribuzione del carico !! grazie a server replicati e DNS

## gerarchia server DNS
i primi 2 livelli sono stati creati per rendere la ricerca veloce !


### DNS caching

i resource record vengono iniviati all’interno dei messaggi DNS