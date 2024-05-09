per implementare la pipeline, e permettere quindi il forwarding, è necessario integrare dei registri tra le varie unità funzionali, in modo tale da poter recuperare un dato se necessario
![[Pasted image 20240509220559.png]]

## modifica write back
in questo “snapshot” della cpu, si nota ci sarebbe un problema con il writeback della prima istruzione, in quanto in quel ciclo di clock verrebbe decodificata l’istruzione 4. per questo motivo il write back della prima istruzione avverrebbe nel registro sbagliato ! (il reg destinazione dell’istr 4)
![[Screenshot 2024-05-09 221907.png]]

![[Pasted image 20240509221854.png]]