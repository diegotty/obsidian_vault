## fetch della istruzione / aggiornamento PC
il seguente blocco, gestisce l’avanzamento del PC e la lettura dell’istruzione dalla memoria delle istruzione
note:
- le connessioni sono da 32bit
- mentre viene letta l’istruzione viene già calcolato il nuovo PC
![[Pasted image 20240420123000.png|400]]
## operazioni ALU e accesso a MEM
decodifica facilitata: i formati I ed R sono quasi uguali
il secondo argomento dell’istruzione può essere o un registro o un campo immediato. per gestire ciò si usa un MUX