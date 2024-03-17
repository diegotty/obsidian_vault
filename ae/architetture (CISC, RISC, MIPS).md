## architettura cisc vs risc

| architettura CISC                                                                                                                               | architettura RISC                                                                                                         |
| ----------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **istruzioni di dimensione variabile**<br>-per il fetch della successiva serve capire(decodificare) la dimensione di quella prima               | **istruzioni di dimensione fissa**<br>-fetch della successiva senza decodifica della precedente                           |
| **formato variabile**<br>-decodifica complessa                                                                                                  | **formato uniforme**<br>-decodifica ez                                                                                    |
| **operandi in memoria**<br>-bisogna accedere spesso alla ram                                                                                    | **operazioni ALU solo tra registri**<br>-no accesso in memoria                                                            |
| **pochi registri interni**<br>- più accessi in memoria                                                                                          | **molti registri interni**<br>-risultati parziali senza accesso in memoria (?)                                            |
| **modi di indirizzamento complessi**<br>-più accessi in memoria<br>-durata variabile della istruzione<br>-conflitti tra istruzioni + complicati | **modi di indirizzamento semplici**<br>-1 solo accesso in memoria<br>-durata fissa dell’istruzione<br>-conflitti semplici |
| **istruzioni complesse**                                                                                                                        | **istruzioni semplici**                                                                                                   |
>[!tuff] MIPS è RISC !

