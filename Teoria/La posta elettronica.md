# La posta elettronica
## fasi della progettazione di un servizio
- analisi dei requisiti
- specifica delle primitive di servizio
- definizione dell’architettura e delle operazioni
- definizione dei protocolli




### analisi dei requisiti
- gestione di messaggi in partenza
- spedizione di messaggi ad uno o più destinatari
- ricezione di messaggi in arrivo
- gestione messaggi ricevuti
- preservazione della riservatezza dei dati
- certezza della consegna (si ammette anche una consegna differita nel tempo)
- configurazione poco onerosa da parte degli utenti finali
- interfaccia utente elementare ed intuitiva

### primitive di servizio
- gestione messaggi in partenza
- gestione messaggi ricevuti

#### connessione diretta mittente-destinatario
- il mittente non può spedire il messaggio se il destinatario non ha posto in esecuzione il suo processo, oppure ha spento il computer
- il mittente deve ricordare il nome della macchina del destinatario
- non è chiaro come si possa autenticare il destinatario

#### server del “dominio” del destinatario
- il server può essere guasto o impegnato, e il mittente può chiudere il suo programma prima che il messaggio sia recapitato

#### server per l’invio e server in ricezione

### indirizzi di posta elettronica
un indirizzo di email è composto da due parti, separate dal simbolo @
- la prima parte identifica un utente
- la seconda parte è il nome di un dominio

## tipologia e molteplicità delle applicazioni
due tipi di applicazioni sono coinvolte nel servizio:

### MUA (Mail User Agent)
- detto anche mailer, implementa un’interfaccia per l’utente
- l’utente lo manda in esecuzione quando vuole accedere al servizio di posta elettronica (e lo può interrompere subito dopo aver usato le primitive che gli interessano)

> pagina Web attraverso la quale si accede a Gmail ...

### MTA (Mail Transmission Agent)
- è un’applicazione che fa da intermediaria nel processo di trasmissione del messaggio dalla sorgente fino alla destinazione
- il servizio è offerto in maniera il più possibile continuativa e stabile nel tempo

## architettura
ruolo dei server che ospitano un MTA:
### Outgoing Mail Server
il computer il cui MTA è quello a cui fa diretto riferimento il MUA per l’inoltro della posta

### Mail eXchanger
ogni dominio definisce nel DNS una lista di host (in ordine di priorità) che ospitano gli MTA che sono incaricati di ricevere posta per quel dominio

### Incoming Mail Server
l computer che ospita l’MTA da cui il MUA ritira la posta; generalmente coincide con il Mail eXchanger primario del dominio

### in dettaglio
- talvolta il MTA che è ospitato sull’outgoing mail server è suddiviso in due processi, il primo si chiama MSA (Message Submission Agent) ed è quello a cui il MUA si rivolge per spedire la posta ed il secondo è il vero e proprio MTA, che cura la spedizione al MTA del dominio del destinatario
- lo stesso avviene sull’ incoming mail server, nel quale si distinguono due processi, il primo è l’MTA che riceve la posta dal dominio del mittente mentre il secondo si chiama MDA (Mail Delivery Agent) ed è responsabile del recapito all’utente


- il MUA trasmette il messaggio al suo Outgoing Mail Server
- il Mail Server richiede (tramite il DNS) la lista dei Mail eXchanger del dominio destinazione (in ordine di priorità) e cerca di trasmettere il messaggio ad uno di essi; se fallisce con tutti, salva la mail nel suo file system e riprova ad inoltrarla ad intervalli di tempo regolari; se fallisce per tre giorni consecutivi notifica al mittente il fallimento
- un Mail eXchanger secondario cerca ad intervalli di tempo regolari di trasmettere il messaggio al Mail eXchanger primario

### servizi ausiliari
DNS (Domain Name System): necessario all’Outgoing Mail Server per ottenere la lista e la priorità dei Mail eXchanger del dominio destinazione

File System Distribuito: se l’incoming mail server non coincide con il Mail eXchanger primario del dominio destinazione, l’incoming mail server deve poter accedere ai file delle mail tramite un sistema per il file system distribuito

```
nslookup
dig -t MX <domain>
```

#### primo tipo di flusso di informazione
#### secondo tipo di flusso di informazione

### definizione delle comunicazioni
data la diversità delle due tipologie dei flussi di informazione si decide di ricorrere a due sistemi di comunicazione differenti

> per entrambi i sistemi si riconosce la necessità di trasferire una quantità di dati non quantificabile a priori (un messaggio può essere arbitrariamente lungo) è conveniente, dunque, istituire in entrambi i casi un canale di comunicazione connesso (uso di TCP su IP)
>
> il progetto delle tipologie di comunicazione si riduce dunque alla definizione del protocollo relativo al trasferimento dei messaggi fino al server del dominio di destinazione (smtp) e del protocollo relativo al trasferimento del messaggio dal server del dominio destinazione al MUA del destinatario (per esempio pop3)

### aspetti della definizione di un protocollo
- Protocol Data Unit (descrizione del formato delle informazioni che vengono trasmesse)
   - richieste del client (comandi + argomenti)
   - risposte del server 
- procedure
   - le regole d’uso del protocollo, le regole sintattiche, l’effetto dei comandi
- stati
   - gli stati che le due applicazioni assumono a seguito dei comandi emessi o delle risposte ricevute, i comandi ammessi in ogni stato

## SMTP - simple mail transfer protocol
> rfc 821 e 5321 e 5322
>- port TCP 25
>- protocollo molto semplice, text based
>- il body dei messaggi di posta elettronica è definito nella rfc

- HELO <dominio>
   - comando di apertura: inizia qualsiasi conversazione in smtp
- MAIL FROM:<mittente>
   - iniza il trasferimento di una mail per uno o più destinatari
- RCPT TO:<destinatario>
   - individua un destinatario
- DATA
- RSET
   - reset: il trasferimento corrente viene abortito 
- VRFY <destinatario>
   - richiede al ricevente di confermare che il destinatario esiste
- EXPN <alias>
   - richiede al ricevente di confermare che l’alias di posta specificato
- HELP
   - mostra tutti i comandi disponibili 
- HELP <comando>
- NOOP
   - non fa nulla oltre a richiedere una risposta affermativa alla controparte 
- QUIT
   - termina la comunicazione

|Code|meaning|
|:-:|:-|
|500| Command unrecognized|
|501| Syntax error in parameters|
|502| Command not implemented|
|503| Bad sequence of commands|
|504| Command parameter not implemented|
|211| System status, or system help reply|
|214| Help message|
|250| Requested mail action okay, completed|
|552| Requested mail action aborted: exceeded storage allocation|
|553| Requested action not taken: mailbox name not allowed|
|354| Enter mail, end with "." on a line by itself|
|554| Transaction failed|

- la transazione è iniziata dal comando MAIL che deve essere seguito dal comando RCPT (eventualmente iterato) e poi dal comando DATA
- Il server entra in uno stato di accettazione del messaggio che termina nel momento in cui riceve la sequenza <CRLF>.<CRLF>
- nessun comando viene accettato durante il trasferimento del messaggio
- la conversazione è terminata dal comando QUIT

```
telnet <domain> 25
```

### post office protocol 3
- usa la porta TCP well known 110
- specificato nelle rfc 1725 e 1939
- i comandi del pop3 sono composti di quattro lettere
- le risposte del server sono precedute da un +OK se positive o da un -ERR se negative
- l’uso di questi comandi è esemplificato nelle pagine seguenti

>- USER username
>- PASS password
>- STAT
>- LIST
>- LIST numero messaggio
>- RETR numero messaggio
>- DELE numero messaggio
>- NOOP
>- RSET
>- QUIT

### pop3
una sessione pop3 attraversa vari stati:
- quando la connessione TCP è stata instaurata, pop3 entra nello stato AUTHORIZATION - in questo stato il client deve identificarsi
- quando ciò è avvenuto, il server acquisisce le risorse associate al client e la sessione entra nello stato TRANSACTION - in questo stato il client richiede servizi da parte del server
- quando il client spedisce il comando QUIT, la sessione entra nello stato UPDATE - in questo stato il server rilascia le risorse e la connessione TCP viene chiusa
