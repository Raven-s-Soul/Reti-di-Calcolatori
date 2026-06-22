# Gli switch-bridge
>[!TIP]
> i bridge sono pensati per realizzare topologie di reti locali articolate e con molti computer (in ciò che segue bridge e switch sono sinonimi)
>> - le schede del bridge alle quali sono collegati i fili si chiamano porte
>> - i bridge possono anche essere collegati fra loro, consentendo di realizzare reti locali con topologie articolate
>> - un bridge cerca di ritrasmettere solo i pacchetti che devono effettivamente transitare da una parte della LAN a un’atra parte della LAN: funzione di filtering
>> - i bridge ritrasmettono i pacchetti ricevuti con modalità *store & forward* (memorizza e ritrasmetti)

### caratteristiche generali dei bridge
- livello 2 della pila ISO-OSI, sottolivello MAC dello standard IEEE 802
- algoritmi di instradamento semplici e locali (routing isolato)
- i bridge devono essere conformi allo standard IEEE 802.1D, hanno tabelle di instradamento a bordo e i sistemi sulle LAN ne ignorano la presenza
 
## bridge, MAC e porte

> le porte di un bridge possono avere lo stesso MAC o avere MAC differenti
> - nel caso in cui un pacchetto sia ricevuto da una
porta con un MAC e ritrasmesso da una porta con
un altro MAC occorre effettuare una traduzione di
formato del pacchetto, compreso il ricalcolo
dell’FCS
>- se l'interconnessione è con parti di LAN non
conformi a IEEE 802, allora occorre preoccuparsi
anche di LLC

>[!NOTE]
>i bridge costruiscono la propria tabella di instradamento (detta anche filtering database) con un processo di learning
>> il processo di learning funziona solo se la topologia della LAN è ad albero
>>
>> la topologia è normalmente a grafo (contiene uno o più cicli), per motivi di affidabilità
>> 


## prestazioni dei bridge

influenzano le prestazioni dell’intera rete locale
- numero massimo di pacchetti al secondo processabili
- tempo medio di latenza: tempo di attraversamento del bridge da parte di un pacchetto (dall’ingresso del primo bit alla sua ritrasmissione)
- è preferibile che un bridge sia full speed: parametri pari al massimo teorico
- difficoltà: più corti sono i pacchetti e più è alto il numero di decisioni da prendere nell'unità di tempo
- il tempo di latenza è anche funzione della lunghezza del pacchetto

### architettura fisica
- cpu + memoria + interfacce per le varie LAN
- in ROM le funzionalità dello standard IEEE 802.1D, in RAM le tabelle di instradamento, i buffer dati e le strutture di dati ausiliarie
- alternativa: schede ASIC (Application Specific Integrated Circuit), in grado di risolvere localmente parte dell’instradamento

### architettura logica
- due o più porte
- MAC relay entity, per ritrasmissione, filtraggio, learning
- entità di livello superiore, higher layer entities, per il calcolo dello spanning tree ed il controllo generale del bridge

## stato delle porte
>l’amministratore di rete può mettere ogni porta in stato di enabled (attiva) o disabled
>- una porta attiva può essere in stato di forwarding o di blocking, a causa dell’algoritmo di spanning tree
>- le porte hanno un indirizzo MAC e sono numerate progressivamente nel bridge a partire da 1, l’indirizzo del bridge è uguale all'indirizzo MAC della porta 1

## tabella di instradamento
- la tabella contiene entry (righe) statiche ed entry dinamiche
- il processo di learning si basa sugli indirizzi mittente dei pacchetti ascoltati
- il valore di default per la sopravvivenza delle entry dinamiche è 5 minuti

>[!IMPORTANT]
>## controllo di flusso – IEEE 802.3x e 802.3bd
>
> Esmpio di un output enorme e di un imput minore, può provare saturazione dell buffer dello switch
>
>introduzione del pause frame: MAC control frame di 512 bit con:
>- MAC-dsap multicast (1-80-c2-00-00-01)
>- length/type = 88-08
>- pause time
>
>- la presenza di pacchetti MAC che non portano dati ma che contengono solo informazioni di controllo (control frame) è una novità (ce ne sono varie tipologie in altri MAC: es wi-fi)
>- viene introdotto un nuovo sottostrato di MAC denominato MAC control
>- il supporto a 802.3x è opzionale e viene negoziato tra le schede alle due estremità del filo








