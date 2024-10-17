---
created: 2024-10-17
related to: "[[progettazione, problemi e vincoli#dipendenze funzionali]]"
updated: 2024-10-17, 07:24
---
//TODO index
dato uno schema R e un insieme di dipendenze funzionali F, un’istanza legale di R oltre a soddisfare l’insieme di dipendenze funzionali F, soddisfa anche un insieme di dipendenze più grande: $F^+$
$$F \subseteq F+$$
$F^+$ è la chiusura di F, cioè l’insieme di dipendenze funzionali che sono soddisfate da ogni stanza legale di R
# chiave e superchiave
dato uno schema R e un insieme di di dipendenze funzionali F, un sottoinsieme K dello schema R è una chiave di R se:
1. $K→R \in F^+$
2. non esiste un sottoinsieme **proprio** $K'$ di $K$ tale che $K’→R \in F^+$
con $K→R$ si intende che il sottoinsieme K determina l’intera istanza
se il sottoinsieme K soddisfa solo la proprietà 1, allora K è una superchiave di R.
per essere una chiave, la proprietà 2 deve essere soddisfatta (non devono esistere sottoinsiemi di K che sono a loro volta chiavi)
>[!example] Studente = Matr Cognome Nome Data
>- il numero di matricola viene assegnato allo studente per iden