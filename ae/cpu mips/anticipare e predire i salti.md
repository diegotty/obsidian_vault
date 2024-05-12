# ancitipare il jump
## perchè
la decisione di eseguire il jump viene presa dalla CU nella fase ID. nel frattempo, un’altra istruzione è stata già caricata (e potrebbe essere necessario annullarla.)
![[Pasted image 20240512222805.png]]
lo stallo necessario per eliminare l’istruzione implica annullare l’istruzione inutile e aggiornare il PC alla destinazione corretta.
## implementazione anticipo
per spostare il jump nella fase IF serve:
- anticipare il riconoscimento dell’istruzione(comparatore con il valore dell’OpCode del jump, 00010)
- spostare la logica di aggiornamento del PC alla fase IF
![[Screenshot 2024-05-12 (22.31.58).jpeg.png|300]]
# anticipare beq
## perchè
l’istruzione beq normalmente usa la ALU per fare il confronto, per cui:
- il salto avviene dopo la fase EXE (nella fase MEM), e in caso di salto, le 2 istruzioni seguenti (già caricate) vanno annullate
- la necessità di avere gli argomenti nella fase EXE porta ad aver bisogno di uno stallo se il salto è preceduto da una lw




# annullare le istruzioni 
per annullare le istruzi