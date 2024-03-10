<h1> Titolo </h1> 
<h3>Esplorazione dei dati dell'accelerometro: confronto tra camminate con e senza plantari </h3> <br>
<h1> Obiettivi </h1> <br>
Lo scopo di questo progetto è quello di confrontare i dati acquisiti <i>( tramite l'applicazione <b>Sensor Logger</b> <i>(https://github.com/tszheichoi/awesome-sensor-logger)</i>) </i>da diversi percorsi della stessa durata (~ 2 minuti), con e senza l'utilizzo di plantari ortopedici .<br> In particolare,l'analisi è stata effettuata sul sensore accelerometro, per esaminare le variazioni nell'accelerazione durante le attività. 
<br><br><br>Gli obiettivi principali che mi sono posto sono:<br>
<li> Comparare le caratteristiche dell'accelerazione registrate durante la camminata su tapis roulant con e senza l'uso di plantari ortopedici </li>
     <li> Comparare le caratteristiche dell'accelerazione registrate durante la salita e la discesa delle scale,  con e senza l'uso di plantari ortopedici </li>  
 <li> Ridurre il rumore dei dati rilevati tramite l'uso di un filtro (passa-basso) </li>  <li> Esplorare e confrontare eventuali differenze rilevate </li> 
 <br> <br>
<h1> Teoria e metodi </h1>

Il sensore utilizzato per questa analisi è l'accelerometro.

Ho applicato varie tecniche di analisi dati, tra cui calcoli delle medie, identificazione di picchi e valli e l'applicazione di un filto passa-basso.
    
Per esplorare e visualizzare i dati raccolti, ho importato tramite la libreria <b> pandas</b> i file .csv acquisiti da analizzare (convertiti in timestamp). Ho utilizzato inoltre Plotly per generare plot dinamici ed interattivi dei dati

<h3> <li>  SPIEGAZIONE E MOTIVAZIONE DELL'IDENTIFICAZIONE DI PICCHI E VALLI </li> </h3> 

Tramite la funzione <b>find_peaks</b>, ho prima individuato i picchi positivi nell'accelerazione. Ho analogamente in seguito individuato le valli <i>(questa volta negando il segnale)</i>. La distanza minima tra i picchi è stata impostata a 25 campionamenti per evitare di rilevare picchi troppo vicini tra loro come separati <i>(ragionamento identico applicato poi anche per le valli)</i>. 
Tramite la funzione Plotly ho reso possibile la visualizzazione grafica come plot.
<br><br>
Sono stati infine calcolati i valori medi di picchi <i>(e valli)</i>, al fine di fornire una stima del comportamento medio durante il periodo di rilevamento dei picchi e delle valli. 

<br><br>

<h3> <li> SPIEGAZIONE E MOTIVAZIONE DEL CALCOLO DELLE FREQUENZA MEDIE </li> </h3>  <BR> 
Al fine di garantire coerenza nell'analisi dei dati, ho deciso di calcolare una frequenza di campionamento media.
La funzione <b> df_acce_with['time'].diff()</b> calcola la differenza temporale tra le misurazioni consecutive, fornendo gli intervalli di campionamento, prima per i dati raccolti con i plantari e dopo quelli senza.
<br><br>

Il calcolo della variabile <b>sampling_frequency</b> <i>(ed ovviamente il ragionamento si applica poi per la variabile <b> sampling_frequency_without </b>)</i> mi permette di ottenere la frequenza di campionamento desiderata.

<br><br>

<h3> <li>TEORIA, SPIEGAZIONE E MOTIVAZIONE DELL'UTILIZZO DEL FILTRO BUTTERWORTH  </li> </h3>  <BR> 
Allo scopo di ridurre il rumore nei dati raccolti, ho applicato un filtro passa-basso; nello specifico il filtro <b>Butterworth</b>. 
<br><br>
<img src="Filtro Butterworth.png" alt="Filtro Butterworth" width="350" height="450"/>
<br><br>
Per quanto riguarda i parametri del filtro <b>butter_lowpass</b>, troviamo "<b>cutoff_frequency</b>" che rappresenta la frequenza di taglio del filtro e "<b>order</b>", che rappresenta l'ordine del filtro Butterworth.

Reputo i valori scelti per questi due parametri adeguati ad una pulizia sufficente da non cambiare in modo significativo il segnale
<br><br>
<h3> <li>SPIEGAZIONE E MOTIVAZIONE DEL CALCOLO MEDIE E DIFFERENZE CON SEGNO</li> </h3>
Per comprendere meglio le caratteristiche del segnale, ho pensato di osservare come l'accelerazione varia lungo l'asse x rispetto alla media, sia con che senza l'uso dei plantari.

Ho applicato lo stesso ragionamento anche sul segnale filtrato.

Tramite le funzioni <b> mean_x_with = df_acce_with['x'].mean() </b> e <b> mean_x_without = df_acce_without['x'].mean() </b> ho calcolato le medie dei dati lungo l'asse x, mentre con le due seguenti sottrazioni <i> (<b> diff_with = df_acce_with['x'] - mean_x_with </b> e <b> diff_without = df_acce_without['x'] - mean_x_without</b>)</i>, vengono calcolate le differenze con segno rispetto alla media per i dati con e senza l'uso di plantari, in modo tale da valutare quanto ogni campione si discosta dalla media. 
</h6>


<h1> Conclusioni </h1>
Riepilogando i dati analizzati, non vengono evidenziate (a mia sorpresa) grandi differenze; nelle specifiche camminate analizzate, l'uso dei plantari non sembra influire in maniera significativa sulle accelerazioni rilevate.

Per capire a fondo e rilevare eventuali differenze importanti, vorrei far notare che:

<ul> <li> I plantari in questione sono utilizzati da diversi anni; sarebbe stato molto più interessante fare questo confronto durante i primi utilizzi.  </ul> </li> 
<ul> <li> La breve durata delle camminate e la semplicità dei percorsi potrebbero aver limitato la rilevazione di alcune differenze; un'analisi più completa <i>(per esempio tramite l'utilizzo di altri sensori oltre all'accelerometro)</i> potrebbe far emergere miglioramenti della stabilità del passo grazie all'utilizzo dei plantari. </ul> </li> 
