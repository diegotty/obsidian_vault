## fetch della istruzione / aggiornamento PC
il seguente blocco, gestisce l’avanzamento del PC e la lettura dell’istruzione dalla memoria delle istruzione
note:
- le connessioni sono da 32bit
- mentre viene letta l’istruzione viene già calcolato il nuovo PC
![[Pasted image 20240420123000.png|400]]
## operazioni ALU e accesso a MEM
decodifica facilitata: i formati I ed R sono quasi uguali
il secondo argomento dell’istruzione può essere o un registro o un campo immediato. per gestire ciò si usa un MUX: la linea di controllo `ALUSrc` seleziona  **dato letto 2** o **l‘output del blocco di estensione del segno**

note: 
nel blocco di estensione del segno entrano i 16bit non letti dal blocco dei registri(che corrispondono all’indirizzo del valore immediato delle istruzioni di tipo I)
## esempi
![[Pasted image 20240420125612.png]]
//add last 2

## salti condizionati (beq)
struttura per poter effettuare salti condizionati:
![[Pasted image 20240421140628.png|450]]

parte della struttra completa (senza la linea di controllo `PCSrc`)
![[Pasted image 20240421140430.png|500]]
struttura completa: 
viene usata una porta and per controllare se:
- l’istruzione è un branch (la linea viene presa dal blocco [[#unità di controllo]])
- i due registri sono uguali (siamo implementando un `beq`)
l’uscita della porta and viene poi usata come linea di selezione per il mux
![[Pasted image 20240421140936.png|500]]

## unità di controllo

| ALU control lines | function         |
| ----------------- | ---------------- |
| 0000              | AND              |
| 0001              | OR               |
| 0010              | add              |
| 0110              | subtract         |
| 0111              | set on less than |
| 1100              | NOR              |
