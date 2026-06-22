# ICMPv4
il protocollo IPv4 ha un approccio best effort (sforzo migliore)
- non è in grado di garantire la consegna dei pacchetti, ma esegue dei tentativi, al meglio delle possibilità di cui dispone
- alcuni pacchetti possono essere scartati da IPv4: sono "dropped on the floor", "lasciati cadere in terra"

## ICMP - Internet Control Message Protocol
>ICMP offre, a supporto di IPv4, un semplice servizio di error-reporting
>- protocollo per i messaggi di controllo in Internet
>
>ICMP consente anche di spedire messaggi per richiedere, ed eventualmente ottenere, informazioni

### Richiami
- Hop (Salto): un computer intermedio tra la sorgente e la destinazione del pacchetto
- tempo di vita: (ttl, "time to live") misura del tempo residuo di validità del pacchetto
   - conta gli hop rimanenti
   - ogni router decrementa il ttl di 1
   - i pacchetti con ttl=0 vengono scartati

### ICMP - regole
>  RFC 792
>
>- regola 1: nessun messaggio ICMP viene generato a seguito di eventuali errori rilevati su altri messaggi ICMP
>- regola 2: se il pacchetto viene frammentato, allora solo il primo frammento può generare messaggi di errore ICMP
>- regola 3: i pacchetti broadcast e multicast non generano ICMP

### imbustamento

|frame header|ip header|ICMP hdr| ICMP data |
|:-:|:-:|:-:|:-:|

|frame header|ip header|ip data|
|:-:|:-:|:-:|

|frame header|frame data|
|:-:|:-:|


### messaggi di destination unreachable
- network unreachable: un gateway vede la rete a cui è destinato il pacchetto a distanza infinita
- host unreachable: l'host a cui è destinato il pacchetto non risponde ad una chiamata ARP
- protocol unreachable: l'host a cui è destinato il pacchetto non conosce il protocollo nel pacchetto
- port unreachable: la port specificata non è raggiungibile
- fragmentation needed and DF set: il pacchetto non può essere frammentato

### messaggi di tempo scaduto
- TIME_EXCEEDED (tempo scaduto)
   - il pacchetto ha TTL=0 e quindi viene scartato

### messaggi di redirection
- REDIRECT (ridireziona) un router rileva che il prossimo router cui dovrebbe inoltrare il pacchetto è sulla stessa LAN del mittente

### messaggi di eco
- ECHO_REQUEST e REPLY (richiesta di eco e relativa risposta)
- TIMESTAMP e TIMESTAMP_REPLY come ECHO, più informazioni su orario invio

## il comando ping
ha l’obiettivo di verificare se un indirizzo IP è raggiungibile e qual è il ritardo per raggiungerlo
- invia una sequenza di pacchetti ICMP ECHO_REQUEST e attende la relativa risposta ECHO_REPLY
- misura il tempo che intercorre tra l’invio della richiesta e la ricezione della risposta per ogni pacchetto
- riporta semplici statistiche

```
ping <ip>
tcpdump -n 'icmp' // osservati 
ping 193.204.162.255 // broadcast
```

## traceroute
ha l’obiettivo di verificare quali router sono attraversati per raggiungere un dato indirizzo IP
```
traceroute <ip>
```



