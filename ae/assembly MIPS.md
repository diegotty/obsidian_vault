# syntax
## main tags
- .data
- .text
types:
- .asciiz
- .byte
- .float
- .half
- .word
## etichette
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
>[!tuff] le etichette sono fondamentali per le strutture di controllo del flusso !!

# strutture di controllo di flusso
## if-else

```c
if (x > 0) {} 
else {}
```

```arm-asm
.text
# uso il registro $t0 per la var X

blez $t0,else            # branch (if) less than zero
	#codice per condizione vera
	j endIf              # jump to endIf(label), altrimenti esegue else: labeled code
else:
	#codice per condizione falsa
endIf:
	#stuff

```

***
## do while

```c
do {
	// codice da ripetere se condizione vera
	// il corpo del ciclo DEVE aggiornare x
} while (x != 0)
```

```asm-arm
.text
# uso il registro $t0 per indice x

do:
	# codice da ripetere
	
	bnez $t0,do      # branch (if) not equal zero
	# code out of loop
```

***
## while
```c
while (x != 0) {
	// codice da ripetere se x != 0
	// il corpo del ciclo DEVE aggiornare x
}
```

```asm-arm
.text
# uso il registro $t0 per indice x

while:
	beqz $t0,endWhile    # esci se x = 0
	# codice da ripetere
	j while
endWhile:
	# codice seguente
```