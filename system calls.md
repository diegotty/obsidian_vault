## syscall
richieste al sistema operativo
 - input:
	 - `$v0` : operazione richiesta
	 - `$a0,.., $a2, $f0` : eventuali parametri
- output:
	- `$v0, $f0` : eventuale risultato



| syscall($v0) | service      | argomenti($a0, ..) | risultato($v0, …) |     |
| ------------ | ------------ | ------------------ | ----------------- | --- |
| 1            | print_int    | `$a0` = integer    |                   |     |
| 2            | print_float  | `$f12` = float     |                   |     |
| 3            | print_double | `$f12` = double    |                   |     |
| 4            | print_string |                    |                   |     |
| 5            | read_int     |                    |                   |     |
| 6            | read_float   |                    |                   |     |
| 7            | read_double  |                    |                   |     |
| 8            | read_string  |                    |                   |     |
| 9            | sbrk         |                    |                   |     |
| 10           | exit         |                    |                   |     |
| 11           | print_char   |                    |                   |     |
| 12           | read_char    |                    |                   |     |
| 13           | open         |                    |                   |     |
| 14           | read         |                    |                   |     |
| 15           | write        |                    |                   |     |
| 16           | close        |                    |                   |     |
| 17           | exit2        |                    |                   |     |
|              |              |                    |                   |     |