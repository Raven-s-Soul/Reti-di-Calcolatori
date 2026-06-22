# DNS

esiste un meccanismo che permette di denotare un’interfaccia di rete specificandone il nome (più facile da ricordare), e che si occupa della traduzione del nome in un indirizzo IP (mapping)
- l’insieme dei nomi ammissibili è detto namespace
- inizialmente in Internet si utilizzava un namespace "flat", in cui i nomi erano sequenze di caratteri, senza alcuna struttura
- inoltre la possibilità di aggiungere e modificare i nomi competeva a un’unica autorità centrale, che doveva occuparsi anche di conservare la coerenza tra le informazioni duplicate

### impostazione generale del servizio
si è pensato quindi di
- decentralizzare il sistema di assegnazione dei nomi
- decentralizzare la responsabilità del mapping nome-indirizzo
- realizzare l’accesso al mapping con tecniche client-server

uso di un database distribuito di corrispondenze nome-indirizzo
- robustezza ed efficienza conseguite anche mediante:
- replicazione
- caching

>[!NOTE]
> i nomi sono sequenze di caratteri separate da punti, l’albero del namespace

- il livello più alto (top level) della gerarchia partiziona lo spazio dei nomi e delega la gestione dei nomi delle diverse partizioni ad autorità locali
- l’autorità top level non deve occuparsi di questioni interne alle partizioni
- la gerarchia viene definita non (o non solo) in base alla collocazione fisica degli host nella rete, ma in accordo con la struttura dell’organizzazione a cui appartengono

- ci sono alcuni (circa 1.500) domini top-level
- per generalità, si assume che tutti i domini top-level siano figli di un unico dominio radice, cui corrisponde la stringa vuota ("")
- attenzione: la struttura del namespace è largamente ortogonale a quella delle sottoreti di IP


- l’organizzazione dei nomi di Internet è detta Domain Name System (DNS)
- i sistemi che realizzano i mapping tra nomi ed indirizzi sono detti name server (ns)
- alcuni name server hanno la delega per una porzione del namespace
- i name server possono dialogare tra loro

### zona
- un name server ha informazioni complete su una parte del namespace detto zona; quel name server è l’autorità per quella zona
- i concetti di dominio e zona sono differenti
- un name server può essere l’autorità per varie zone
- alla stessa zona possono essere associati vari name server autorità

### name server – primary e secondary
relativamente ad una zona i name server possono essere
- primary
   - nei suoi file di configurazione, la versione corretta e aggiornata delle informazioni di mapping della zona 
- secondary
   - richiede periodicamente al primary una copia aggiornata delle informazioni di mapping della zona

> un name server può essere contemporaneamente primary per certe zone e secondary per altre

## resolver
i client che usano i name server si chiamano resolver

i resolver sono a bordo degli host e sanno
- interrogare un name server
- interpretare le risposte
- inviare le informazioni ricavate ai programmi che li utilizzano

> non è detto che un resolver sia un processo autonomo

### atteggiamenti possibili nella risoluzione
la risoluzione può essere ricorsiva o iterativa
- ricorsiva: il client chiede a quale indirizzo corrisponda il nome n e pretende come risultato l’indirizzo; se il server non possiede l’indirizzo di n è suo compito contattare altri server per ottenerlo
- iterativa: il client chiede a quale indirizzo corrisponda il nome n e, in caso il server non lo abbia, si accontenta di un’indicazione di un altro server a cui rivolgersi

### cache
- durante ogni query un name server apprende informazioni sui nomi e sui server che svolgono il ruolo di autorità per le varie zone
- alcuni server memorizzano anche informazioni negative
- anche le applicazioni (es. browser) hanno una cache DNS
- è necessario definire opportunamente il tempo di vita - time to live (ttl) delle informazioni in cache
- talvolta sono chiamati local name server o caching name server o recursive name server
- le informazioni DNS sono memorizzate in resource record; ogni dominio (anche un singolo host) è associato a uno o più resource record

### resource record
- SOA
   - start of authority; contiene informazioni amministrative sulla zona
- A
   - indirizzo IPv4 di un h
- MX
   - specifica il nome dell’host che accetta le mail indirizzate al dominio del record
- NS
   - name server per una zona
- AAAA
   - IPv4
- CNAME
   - canonical name; il nome corrisponde ad un altro nome (alias)


### formato dei messaggi DNS
-due tipi di messaggi, con uguale formato
   - richieste
   - risposte
- tra i campi dell’header
   - QR: indica se il messaggio è una domanda o una risposta
   - RD: recursion desired, indica se le query è ricorsiva
- tra i campi della question section
   - NAME: nome della risorsa richiesta
   - TYPE: tipo del resource record richiesto

>normalmente i messaggi DNS viaggiano, a livello 4, su UDP
>– i client si rivolgono alla well known port UDP 53

## dig: uso del comando

```
dig <dns>

-server is the name or IP address of the name server to query
-name is the name of the resource record that is to ben looked up
-type indicates what type of query is required: ANY, A, MX,…
-p to query a non-standard port number

+nodnssec
+trace
```

chi gestisce i root name server?
- ripe ncc
