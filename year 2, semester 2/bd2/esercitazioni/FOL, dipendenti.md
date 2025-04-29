---
related to: 
created: 2025-03-02T17:41
updated: 2025-04-29T09:43
completed: false
---
Modellare, mediante formule in logica del primo ordine, le seguenti affermazioni, dopo aver progettato opportuni insiemi di simboli di predicato e/o funzione:
Per ogni formula, determinare una interpretazione che sia un suo modello.
1. Tutte le persone hanno almeno un numero di telefono
$V=\{\}, F= \{\text{}\}, P=\{\text{persona/1, telefono/2, usa/2}\}$
FORALL p (
	persona(p) →
	EXISTS t ( telefono(t) AND usa(p,t) )
)

2. Ogni persona ha esattamente un nome
$V=\{\}, F= \{\text{usa/1}\}, P=\{\text{persona/1, nome/1}\}$
FORALL p (
	persona(p) →
	EXITS n (nome(n) AND usa(p, n))
)

3. Non ci sono dipendenti che lavorano in più di due dipartimenti
$V=\{\}, F= \{\text{}\}, P=\{\text{dipendente/1, lavora/2}, \}$
FORALL d (
	dipendente(d) →
	NOT ( 
		EXISTS dip1, dip2, dip2 (lavora(d, dip1) AND 
		lavora(d, dip2) AND lavora(d, dip3) ) )
)

4. Ogni dipartimento ha esattamente un direttore che è una persona.
$V=\{\}, F= \{\text{nome/1}\}, P=\{\text{dipartimento/1}\}$
FORALL dip (
	dipartimento(dip) →
	
)

