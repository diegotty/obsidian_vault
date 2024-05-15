per implementare la pipeline, e permettere quindi il forwarding, è necessario integrare dei registri tra le varie unità funzionali, in modo tale da poter recuperare un dato se necessario
![[Pasted image 20240509220559.png]]

## modifica write back
in questo “snapshot” della cpu, si nota ci sarebbe un problema con il writeback della prima istruzione, in quanto in quel ciclo di clock verrebbe decodificata l’istruzione 4. per questo motivo il write back della prima istruzione avverrebbe nel registro sbagliato ! (il reg destinazione dell’istr 4)
![[Screenshot 2024-05-09 221907.png]]
![[Pasted image 20240509221854.png]]
fix: il write back viene preso solamente dalla propagazione dei registri, non viene caricata durante la decodifica dell’istruzione nella fase 2. in questo modo non ci possono essere problemi come l’esempio sopra.
![[Pasted image 20240509222902.png]]

## propagazione dei segnali di controllo
possiamo dividere i 9 segnali di controllo delle fasi in 3 gruppi:
![[Pasted image 20240509223255.png]]
in questo modo, possiamo facilmente propagare anche i segnali di controllo. come si vede dall’imagine, dopo ogni fase, non servono più i segnali di controllo di tale fase e non vengono quindi propagati
![[Pasted image 20240509223407.png]]

## scoprire data hazard 
### data hazard in EXE
supponiamo di avere questa sequenza di istruzioni:
```arm-asm
sub $2, $1, $3
and $12, $2, $5
or $13, $6 ,$2
add $14, $2, $2
sw $15, 100($2)
```
In questo caso, nonostante tutte le istruzioni utilizzino il registro `$2` le uniche istruzioni che avranno il risultato corretto di `sub` saranno `add` e `sw` in quanto le altre due leggerebbero solamente il valore precedentemente immagazzinato in `$2`.

- il risultato dell’istruzione sub, è però, già disponibile al termine della fase EXE. 
- inoltre, le istruzioni `and` e `or` hanno bisogno di `$2` solamente all'inizio della loro fase EXE.
di conseguenza, è possibile eseguire questo frammento di codice senza stalli, semplicemente propagando il dato a qualsiasi unità lo richieda non appena è disponibile

Questo veloce esempio ci fa subito capire quali siano le casistiche che ci portano ad un data hazard in EXE:
1a. $\text{EX/MEM.RegistroRd}=\text{ID/EX.RegistroRs}$
1b. $\text{EX/MEM.RegistroRd}=\text{ID/EX.RegistroRt}$
2a. $\text{MEM/WB.RegistroRd}=\text{ID/EX.RegistroRs}$
2b. $\text{MEM/WB.RegistroRd}=\text{ID/EX.RegistroRt}$

il primo hazard generato nell’esempio è quello tra `sub $2, $1, $3` e `and $12, $2, $5`. l’hazard può essere rilevato quando l’istruzione `and` si trova nella fase EXE, e l’istruzione precedente si trova nella fase MEM.
l’hazard è di tipo 1 : $\text{EX/MEM.RegistroRd}=\text{ID/EX.RegistroRs}=\$2$
