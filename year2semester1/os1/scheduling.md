---
created: 2024-10-15
related to: "[[gestione dei processi]]"
updated: 2024-10-15, 09:32
---
lo scheduling gestisce l’allocazione del tempo di esecuzione tra i diversi processi che ne fanno richiesta contemporaneamente
## obiettivi dello scheduling
### long-term scheduling
- decide quali programmi sono ammessi nel sistema per essere eseguiti
- spesso FIFO, o FIFO “corretto” che tiene conto di criteri come la priorità
- controlla il grado di [[introduzione ai sistemi operativi#multiprogrammazione]]
- più processi ci sono, più è piccola la percentuale di tempo per cui ogni processo viene eseguito
tipiche strategie:
- i lavori batch(non interattivi) vengono accodati il LTS li prende quando lo ritiene giusto
- i lavori interattivi vengono ammessi fino a “saturazione” del sistema
- si prova a mantenere un mix tra I/O-bound e CPU-bound jobs
l’LTS può essere chiamato anche quando termina un processo o quando un processo è idle da troppo tempo
### medium-term scheduling
- gestisce lo swapping dei processi (il passaggio da memoria seconda a memoria principale e viceversa)
- gestisce quindi il grado di multiprogrammazione
# short-term scheduling
chiamato anche [[gestione dei processi#dispatcher]], è lo scheduler eseguito più frequentemente
è invocato sulla base di eventi:
- interruzioni di clock
- interruzioni di I/O
- chiamate di sistema
- segnali
lo STS alloca tempo di esecuzione su un processore per ottimizzare il comportamento dell’intero sistema
## criteri per lo STS
bisogna prima effettuare delle distizioni tra i tipi di criteri:
- criteri per utente: minimizzazione del tempo di risposta (tempo tra una richiesta di computazione e la visualizzazione dell’output)
- criteri per sistema: uso efficiente ed effettivo del processore
- criteri correlati alle prestazioni:
- criteri non correlati alle prestazioni:
### i/o scheduling
//TODO add quando lo facciamo

>[!figure] ![[Pasted image 20241015095636.png]]
>transizioni decise dai vari tipi di scheduler