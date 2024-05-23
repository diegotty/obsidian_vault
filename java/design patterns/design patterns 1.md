# strategy pattern
esempio: 
![[Pasted image 20240523190726.png]]
data questa gerarchia di classi, mi viene chiesto di aggiungere la funzionalità di volare, con il metodo vola().

istintivamente, verrebbe da aggiungere un metodo vola() alla classe Anatra.
- in questo modo modo però, se venisse aggiunta un’altra sottoclasse (per esempio, AnatraDiPlastica), avremmo dei problemi in quanto essa non vola.
- 