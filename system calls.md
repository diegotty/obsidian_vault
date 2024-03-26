## syscall
richieste al sistema operativo
 - input:
	 - `$v0` : operazione richiesta
	 - `$a0,.., $a2, $f0` : eventuali parametri
- output:
	- `$v0, $f0` : eventuale risultato



| syscall($v0) | descrizione    | argomenti($a0, ..)                                                                   | risultato($v0, …) |
| ------------ | -------------- | ------------------------------------------------------------------------------------ | ----------------- |
| 1            | stampa intero  | intero                                                                               |                   |
| 4            | stampa stringa | string address                                                                       |                   |
| 5            | leggi intero   |                                                                                      |                   |
| 8            | leggi stringa  | $a0 = buffer address (indirizzo in memoria)<br>$a1 = numero di caratteri da prendere |                   |
| 10           | fine programma |                                                                                      |                   |