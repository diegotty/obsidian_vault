per aggiungere una nuova istruzione bisogna:
- definire la sua codifica
- definire cosa faccia
- individuare le unità funzionali necessarie (e se sono già presenti)
- individuare i flussi delle informazioni necessarie
- individuare i segnali di controllo necessari
- calcolare il tempo necessario per la nuova istruzione e se modifica il tempo totale
### esempio
aggiungere il jump
il jump assoluto deve spostare il PC a un indirizzo assoluto(invece che relativo(aggiungere con una somma al PC)). per fare ciò bisogna:
- prendere i 26 bit di indirizzo dell’istruzione
- shiftarli di 2 bit
- fare un or con i primi 4 bit di PC + 4(per specificare il banco di memoria in cui bisogna saltare (256mb))