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
fino ad ora abiamo considerato i 4 bit che decidono l’operazione(la linea di selezione dell’ALU) come delle incoginte. L’ALU in realtà fa un totale di 6 operazioni in base alla seguente codifica:

| ALU control lines | function         |
| ----------------- | ---------------- |
| 0000              | AND              |
| 0001              | OR               |
| 0010              | add              |
| 0110              | subtract         |
| 0111              | set on less than |
| 1100              | NOR              |
questi 4 bit più i due generati dal blocco control, formano l’ [[intro a MIPS#rappresentazione dell’istruzione|`opcode`]] .
ciò è molto utile perchè, in caso 
![[Pasted image 20240421141924.png|400]]
in qualche modo entrano 6 bit e escono 9 bit ??? decodificatore ????
viene poi guardato l’`ALUop` che esce dal blocco control:
