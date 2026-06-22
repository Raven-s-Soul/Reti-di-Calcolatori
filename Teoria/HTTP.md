# HTTP
## URL: Uniform Resource Locator

|schema|:|parte dipendente dallo schema|
|:-:|:-:|:-:|

### schema
- telenet
- ftp
- mailto
- http
- https
- news
- nntp
- wais
- file

### parte-dipendente-dallo-schema
sintassi viene denominata Common Internet Scheme Syntax ( “sintassi comune dello schema Internet”)

|//| userid:password@|ind_host| :porta | /path |
|:-:|:-:|:-:|:-:|:-:|

#### userid:password@
- può mancare


#### ind_host
- è l'ip

#### :porta
- quando manca viene presa quella di default


#### /path
- indica un file all’interno del server.

### lo schema di HTTP

|http|:|//| userid:password@|ind_host| :porta | /path |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|

- 80 porta di default

## HTTP
protocollo di livello applicativo per la realizzazione di sistemi distribuiti
- basato su un semplice schema richiesta-risposta nel quale la richiesta è inviata da un client e la risposta è inviata da un server
- un colloquio client-server HTTP è una sessione

### un po’ di storia
- 1996 http1
- 2015 http2
- 2022 http3

## fasi di una sessione HTTP - comportamento di base – HTTP/1.0

1. apertura
1. richiesta
1. risposta
1. chiusura

### principali metodi di richiesta HTTP

- GET (richiede risorsa)
- POST (invia dati)
- HEAD (info su risorsa)
- PUT (sostituzione di file)
- DELETE (cancellazione di risorsa)
- OPTIONS (richiede di conoscere quali opzioni sono disponibili per il trasferimento della risorsa specificata)
  
### codici di stato
- 200 invio risorsa
- 201 creazione risorsa
- 202 accetata richiesta ma non ancora completata

ulteriori azioni sono richieste
- 301 disponibile a un altro indirizzo
- 304 risorsa non modificata

la richiesta è errata e non può essere soddisfatta
- 400 sintassi della richiesta non corretta
- 401 autorizzazione richiesta
- 403 risorsa esistente ma non disponibile
- 404 risorsa non esistente
- 405 metodo non permesso per la risorsa specificata
- 414 nome della risorsa troppo lungo

non può essere soddisfatta una richiesta legittima
- 500 errore interno del server non meglio specificato
- 501 funzionalità non implementata
- 505 versione HTTP non supportata

### alcuni request fields
- accept-charset (insieme di caratteri accettabili)
- cookie
- content-length
- host
- if-modified-since
- if-none-match
- ETag
- set-cookie

### cookie HTTP
dato spedito dal server e memorizzato nel client (browser)
- re-inviato dal client al server ogni volta che il client accede a certe risorse (scope del cookie)
- usato per gestire una forma di "stato" nelle sessioni HTTP
- per memorizzare gli oggetti selezionati da un cliente in un negozio on-line
- per registrare il comportamento di un utente (es. le pagine visitate)
- per supportare l’autenticazione degli accessi

### connessioni persistenti – HTTP 1.1 
> a partire da HTTP 1.1 che sulla stessa connessione TCP un client possa effettuare più richieste consecutive ed ottenere le relative risposte
> - il server ha comunque la possibilità di chiudere la connessione TCP quando lo ritiene opportuno
> - la possibilità di rendere persistenti le connessioni è particolarmente importante se tra TCP e HTTP viene usato uno protocollo che ha l’obiettivo di rendere sicura la connessione (es. TLS – Transport Layer Security)

### HTTPS – HTTP su TLS (e su TCP)
> - HTTP sicuro: la sessione prevede l’uso obbligatorio di TLS
> - lo schema della URL è https
> - la porta di ascolto di default del server è la 443

### connessioni con richieste parallele – HTTP/2
- in HTTP/1.1, per quanto sulla stessa connessione TCP possano essere inviate varie richieste, una nuova richiesta può essere inviata solo dopo aver ricevuto la riposta della richiesta precedente
- in HTTP/2 un browser può inviare varie richieste contemporanee sulla stessa connessione TCP (HTTP pipelining)

### HTTP senza TCP – HTTP/3
- HTTP/3 usa un nuovo protocollo di trasporto, chiamato QUIC, variante di TCP
- QUIC usa pacchetti UDP
- si intrecciano quindi l’evoluzione dei protocolli di trasporto e l’evoluzione di HTTP
- in QUIC il three-way-handshake è mescolato con l’handshake del protocollo sicuro TLS 1.3
