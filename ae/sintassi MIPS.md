### main tags
- .data
- .text
types:
- .asciiz
- .byte
- .float
- .half
- .word
### etichette
```armasm
CheckB:
	slt $t0, $s4, $s1 #stiamo dando per scontato che $t0 è 0 !!!!!
	beq $t0, $zero, CheckC
	or $s4, $zero, $s1 # il massimo ora è B
CheckC:
	slt $t0, $s4, $s2
	beq $t0, $zero, CheckD
	or $s4, $zero, $s2
CheckD: 
	slt $t0, $s4, $s3
	beq $t0, $zero, End
	or $s4, $zero, $s3
End:
	sw $s4, rez
```
ci permettono di saltare ad altri pezzi di codice, facendo riferimento all’etichetta in cui è ‘incapsulato’
[!tuff]le etichette sono fondamentali per le strutture di controllo del flusso !!