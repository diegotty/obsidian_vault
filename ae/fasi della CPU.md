# fase di instruction fetch
in questa fase si preleva la nuova istruzione da eseguire. i componenti necessari sono:
- il PC
- la [[progetto di CPU MIPS#memoria delle istruzioni, PC, adder| memoria istruzioni]], avente in input l’indirizzo di 32 bit in memoria dell’istruzione da prelevare e in output i 32 bit corrispondente a tale indirizzo
![[Pasted image 20240420123000.png|400]]

## fase di instruction decode