---
created: 2024-12-09
related to: 
updated: 2024-12-09T22:40
---
a differenza del problema [[intro alla concorrenza#problema del produttore/consumatore|problema del producer/consumer]], le condizioni da soddisfare sono le seguenti:
- più lettori possono leggere il buffer contemporaneamente
- un solo scrittore può leggere il buffer
- se uno scrittore è all’opera sul buffer, nessun lettore può effettuare letture
inoltre, a differenza del problema producer/consumer, il buffer si accede **per intero**
- il buffer è quindi un’area condivisa **intera**, e non ci sono problemi di buffer pieno/vuoto
## soluzione con precedenza ai lettori
```c
**implementazione con precedenza ai lettori
int readcount;
semaphore x = 1, wsem = 1;
void reader(){
	while(true){
		semWait(x);
		readcount++;
		if(readcount == 1) semWait(wsem);
		semSignal(x);
		READUNIT();
		semWait(x);
		readcount--;
		if(readcount == 0) semSignal(wsem);
		semSignal(x);
	}
}
void writer(){
	while(true){
		semWait(wsem);
		WRITEUNIT();
		semSignal(wsem);
	}
}
void main(){
	readcount = 0;
	parbegin(reader, writer);
}
```

```c
**soluzione con precedenza agli scrittori
int readcount, writecount;
semaphore x =
void writer(){
	while(true){
		semWait(y);
		writecount++;
		if(writecount == 1) semWait(rsem);
		semSignal(y);
		semWait(wsem);
		WRITEUNIT();
		semSignal(wsem);
		semWait(y);
		writecount--;
		if(writecount == 0) semSignal(rsem);
		semSignal(y);
	}
}

void main(){
	readcount = writecount = 0;
	parbegin(reader, writer);
}
```