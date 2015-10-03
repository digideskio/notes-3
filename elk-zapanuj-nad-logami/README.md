# ELK - Zapanuj Nad Logami

ELK - Elasticsearch Logstash Kibana

## Spis tre�ci

1) Logi

2) ELK

3) Praktyka

4) ELK a skalowalno��

5) Przydatne linki

## Logi

### Czym s� logi? Po co je stosujemy? Problemy z logami.

> (...) zapis zawieraj�cy informacj� o zdarzeniach i dzia�aniach dotycz�cych systemu informatycznego, systemu komputerowego czy komputera.
>
> -- *via Wikipedia*

- umo�liwiaj� analiz� dzia�ania systemu, detekcj�:
   - b��d�w - sytuacji wyj�tkowych np. logowanie informacji o wyj�tku
   - nieprawid�owo�ci dzia�ania - niezapisanie przes�anych informacji w bazie danych np. z formularza rejestracyjnego,
   - pr�b i sposob�w w�ama�
- multum log�w, w zale�no�ci od u�ytego stacku technologicznego. Prosta web aplikacja to logi z:
   - Linux Logs (Syslog, Auth Log, FTPD Log, etc.)
   - Apache,
   - PHP,
   - MySQL (slowlog, general log, error log)
   - Application Logs.
- ci�ko si� je przegl�da (Old School way : SSH -> tail, cat, grep, less, awk ...)
- du�y rozmiar, brak sp�jnego formatu pomi�dzy logami - powodzenia przy analizie 100GB nieustrukturyzowanych log�w
- nale�y zadba� o wprowadzenie polityki archiwizacji log�w
- niepotrzebne - z punktu widzenia klienta, nie przynosz� korzy�ci biznesowej a jednak:
  - s�...,
  - developerzy kodz� ```$log->warning('...');```
- nie przegl�damy ich - kto w ci�gu ostatniego miesi�ca sam z siebie rzuci� okiem na logi?

Niew�tpliwie logi s� u�ytecznym narz�dziem, jednak wykorzystywane powinny by� z rozwag�. Dobrym pomys�em jest implementacja funkcjonalno�ci umo�liwiaj�cej aktywowanie/deaktywowanie rejestracji zdarze� do log�w (np. tylko tych z poziomem *DEBUG*).

### Poziomy logowania

Poziomy lgowania zdefiniowane przez *Syslog* (opisane w [RFC 5424](http://tools.ietf.org/html/rfc5424)):

Level | Description | Example
------|-------------|--------
Emergency | system is unusable | *Child cannot open lock file. Exiting*
Alert | action must be taken immediately | *getpwuid: couldn't determine user name from uid*
Critical | critical conditions | *socket: Failed to get a socket, exiting child*
Error | error conditions | *Premature end of script headers*
Warning | warning conditions | *child process 1234 did not exit, sending another SIGHUP*
Notice | normal but significant condition | *httpd: caught SIGBUS, attempting to dump core in ...*
Informational | informational messages | *Server seems busy, (you may need to increase StartServers, or Min/MaxSpareServers)...*
Debug | debug-level messages | *Opening config file ...*

Wykorzysytwane mi�dzy innymi przez:

- standard [PSR-3](http://www.php-fig.org/psr/psr-3/) - definiuj�cy uniwersalny interfejs loggera dla aplikacji napisanych w j�zyku PHP,
- serwer HTTP - [Apache 2](http://httpd.apache.org/docs/2.4/mod/core.html#loglevel).

### Co o logach m�wi literatura?

> Logowanie jest Twoim przyjacielem. Aplikacje powinny logowa� na poziomie *WARNING* za ka�dym razem, kiedy przekraczany jest czas na nawi�zywanie po�aczenia sieciowego lub czas odpowiedzi niebezpiecznie si� wyd�u�a. Powiniene� logowa� na poziomie *INFO* lub, je�li logi s� zbyt rozwlek�e, na poziomie *DEBUG* za ka�dym razem, gdy zamykasz po��czenie. Powiniene� logowa� na poziomie *DEBUG* ka�de po��czenie, kt�re otwierasz, w��czaj�c mo�liwie du�o informacji na temat punktu ko�cowego po�aczenia.
>
> -- *Ci�g�e dostarczanie oprogramowania. Automatyzacja kompilacji, testowania i wdra�ania. Jez Humble. David Farley. Helion 2011.*

> (...)
> - instrukcje rejestracyjne pogarszaj� czytelno�� programu, utrudniaj�c oddzielenie jego zasadniczych instrukcji od konstrukcji pomocniczych;
> - instrukcje rejestracyjne dotkni�te s� t� sam� przypad�o�ci�, co komentarze: gdy kod aplikacji ewoluuje, programi�ci zwykle zapominaj� o ich uaktualnieniu, co czyni je nieadekwatnymi do kontekstu, a to jest gorsze ni� kompletna bezu�yteczno��, bo myl�ce;
> - niezale�nie od tego, ile instrukcji rejestracyjnych znajdzie si� w kodzie aplikacji, zawsze b�d� one niewystarczaj�ce - w przypadku kolejnego debugowania prawdopodobnie pojawi� si� nowe. Pozostawianie ich w kodzie sprawia, �e dwa poprzednie problemy staj� si� jeszcze bardziej wyra�ne.
>
> (...)
> Generalnie, najbardziej u�ytecznymi wpisami w dzienniku s� te na najwy�szym (strategicznym) poziomie, odzwierciedlaj�ce typowe dla aplikacji zdarzenia - jak np. logowanie do serwera HTTP. Niskopoziomowe (taktyczne) wpisy maj� u�yteczno�� raczej ograniczon� czasowo - bo np. pe�ni� rol� pomocn� w poszukiwaniu konkretnego b��du - i celowno�� pozostawienia generuj�cych je instrukcji jest co najmniej w�tpliwa.
>
> -- *Debugowanie. Jak wyszukiwa� i naprawia� b��dy w kodzie oraz im zapobiega�. Paul Butcher. Helion 2009.*

## ELK

*Elasticsearch + Logstash + Kibana*

- trzy niezale�ne od siebie open-sourceowe projekty
- vendor - firma Elastic

*ELK* wspomaga proces indeksowania plik�w log�w (ale nie tylko) w jednym miejscu. Dzi�ki temu mo�liwe jest wyszukiwanie w czasie rzeczywistym, analizowanie danych oraz przygotowywanie wizualizacji z informacji zawartych w logach. Jest szybki w du�ych zbiorach danych, nawet takich zawieraj�cych gigabajty informacji.

![elk-flow.png](elk-flow.png)

### Elasticsearch

*Indexing, storage and retrieval engine*

- serwer bazy danych i jednocze�nie silnik wyszukiwania (full-text search*)
- bazuj�cy na *Apache Lucene*
- real-time - szybkie indexowanie danych, dost�p danych od razu
- skalowalny, rozproszony, wysoko dost�pny
- komunikacja odbywa si� za pomoc� JSON + RESTful API (requesty HTTP - GET, POST, PUT, DELETE)
- brak okre�lonego schematu dla sk�adowanych dokument�w - JSON Documents

#### Terminologia

**Cluster** - grupa nodes
 
**Node** - pojedy�cza instacja *Elasticsearch*

**Index** - grupa dokument�w, powi�zanych ze sob� np:

- TwitterTweets (np. per tag),
- FacebookPosts (np. per day),
- WikipediaArticles.

**Document** - odpowiednik rekordu w RDBMS, JSON ({key:value})

**Shard** - cz�� indexu, kt�ra jest dystrybuowana pomi�dzy nodami, forma kopii zapasowej (w momencie gdy kt�ry� z node jest niedost�pny)

// doda� grafik� obrazuj�c� terminologi�: https://s3.amazonaws.com/media-p.slid.es/uploads/szymontezewski/images/42225/Zrzut_ekranu_2013-07-2_o_20.47.51.png

#### Full-Text Search

- spos�b przeszukiwania danych tekstowych,
- bazuje na analizie poszczeg�lnych s��w danej frazy w przeszukiwanym tek�cie
- zasada dzia�ania:
  - przetworzenie dokumentu tekstowego
  - utworzenie wektora s��w zawartych w dokumencie
  - eliminacja stop-s��w (usuwanie szumu "z, ale, od, tam")
  - lematyzacja lub stemming (sprowadzanie do podstawowej formy: samochody > samoch�d)
  - utworzenie indeksu FTS
- [�r�d�o](http://kni.univ.rzeszow.pl/files/prezentacja_media_SPHINX.pdf)


#### Wyszukiwanie w indexie:

```
$: curl -XGET 'http://localhost:9200/'

$: curl -XGET 'http://localhost:9200/{indexName}/_search?q={field}:{expectedValue}'

$: curl -XGET 'http://localhost:9200/logstash-2015.09.15/_search?q=response:404'
```

Przyk�adowa odpowied�, z ��dania */logstash-2015.09.15/_search?q=response:404*:

```json
{
   "took":8,
   "timed_out":false,
   "_shards":{
      "total":2,
      "successful":2,
      "failed":0
   },
   "hits":{
      "total":2,
      "max_score":1.0,
      "hits":[
         {
            "_index":"logstash-2015.09.15",
            "_type":"apache_access",
            "_id":"AU_PxGxlWi-k5wVY_SrA",
            "_score":1.0,
            "_source":{
               "message":"127.0.0.1 - - [15/Sep/2015:08:49:54 +0200] \"GET /lorem-ipsum HTTP/1.1\" 404 498 \"-\" \"Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:39.0) Gecko/20100101 Firefox/39.0\"",
               "@version":"1",
               "@timestamp":"2015-09-15T06:49:54.000Z",
               "type":"apache_access",
               "file":"/var/log/apache2/access.log",
               "host":"adrian-ubuntu-vm",
               "offset":"10039",
               "clientip":"127.0.0.1",
               "ident":"-",
               "auth":"-",
               "timestamp":"15/Sep/2015:08:49:54 +0200",
               "verb":"GET",
               "request":"/lorem-ipsum",
               "httpversion":"1.1",
               "response":"404",
               "bytes":"498",
               "referrer":"\"-\"",
               "agent":"\"Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:39.0) Gecko/20100101 Firefox/39.0\""
            }
         },
         {
            "_index":"logstash-2015.09.15",
            "_type":"apache_access",
            "_id":"AU_PxHBDWi-k5wVY_SrB",
            "_score":0.5945348,
            "_source":{
               "message":"127.0.0.1 - - [15/Sep/2015:08:50:00 +0200] \"GET /admin HTTP/1.1\" 404 493 \"-\" \"Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:39.0) Gecko/20100101 Firefox/39.0\"",
               "@version":"1",
               "@timestamp":"2015-09-15T06:50:00.000Z",
               "type":"apache_access",
               "file":"/var/log/apache2/access.log",
               "host":"adrian-ubuntu-vm",
               "offset":"10201",
               "clientip":"127.0.0.1",
               "ident":"-",
               "auth":"-",
               "timestamp":"15/Sep/2015:08:50:00 +0200",
               "verb":"GET",
               "request":"/admin",
               "httpversion":"1.1",
               "response":"404",
               "bytes":"493",
               "referrer":"\"-\"",
               "agent":"\"Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:39.0) Gecko/20100101 Firefox/39.0\""
            }
         }
      ]
   }
}
```

### Logstash

*Log input slicer and dicer and output writer*

- pipeline: inputs > filters > outputs
- trzy odpowiedzialno�ci:
  - inputs - agregowanie danych z r�nych �r�de�
    - file, TCP, stdin, syslog, websocket, protok� Lmberjack
  - filters - parsowanie danych do postaci znormalizowanej
    - grok, mutate, geoip, json_encode
  - outputs - odsy�anie przetworzonych danych
    - elasticsearch, redis, mongoDB, irc, slack, file - csv

### Grok

- text matcher - umo�liwia parsowanie tekstu w celu wyodr�bnienia interesuj�cych nas danych
- przetwarzanie nieustrukturyzowanych danych do ustrukturyzowanego formatu
- oko�o 120 gotowych wzorc�w umo�liwiaj�cych wyodr�bnianie:
  - daty, czasu
  - url, host,
  - uuid,
  - log levelu
  - itd. [zobacz wi�cej](https://github.com/elastic/logstash/tree/v1.4.2/patterns)
- mo�emy definiowa� w�asne wzorce

Dost�pny jest debugger online dla Groka: [grokdebug.herokuapp.com](http://grokdebug.herokuapp.com)

```
Input:

  81.112.56.18 GET /page?id=contact 1337

Pattern:
  
  %{IP:client} %{WORD:method} %{URIPATHPARAM:request} %{NUMBER:bytes}

Result:

 client  : 81.112.56.18
 method  : GET
 request : /page?id=contact
 bytes   : 1337

```

### Kibana

*Data Displayer*

- aplikacja webowa umo�liwiaj�ca wizualizacj� informacji zawartych w logach (przetworzonych poprzez filtry Logstash)
- intuicyjny interfejs - nawet dla os�b nie technicznych
- umo�liwia filtrowanie i wyszukiwanie danych
- dost�p do nowych danych w Real-Time
- mo�liwo�� generowania wykres�w, list na podstawie danych - przy u�yciu prostego kreatora
- pulpity - zawieraj�ce widgety - wyrkresy, listy - �atwy spos�b edycji (przeci�gnij opu��, rozszerz)

#### Log Details

![kibana-log-details.png](kibana-log-details.png)

#### Discover Tab

![kibana-discover-tab.png](kibana-discover-tab.png)

#### Visualize Tab

![kibana-visualize-1.png](kibana-visualize-1.png)


![kibana-visualize-2.png](kibana-visualize-2.png)

#### Dashboard

![kibana-dashboard.png](kibana-dashboard.png)

## Praktyka

Opis dotyczy uruchomienia serwera *ELK* oraz konfiguracji serwer�w aplikacyjnych (*LAMP Node* - Linux, Apache, MySQL, App) kt�re za pomoc� *Logstash Forwarder* b�d� przekazywa� logi do serwera *ELK*.

![simple-architecture.png](simple-architecture.png)

### ELK Server

Predefined requirements dla serwera *ELK*:

- Git Client
- Docker & Docker Compose

#### Wygenerowanie certyfikatu SSL

Rekomendowana lokalizacja plik�w:

- certificates (.crt): ```/etc/pki/tls/certs/```
- keys (.key): ```/etc/pki/tls/private/```

Wygenerowane certyfikaty b�d� potrzebne dla *logstash* (pliki .crt oraz .key) i *logstash-forwarder* (tylko plik .crt).

##### Localhost

W przypadku kombinacji *ELK* server jako kontener Dockera + logstash-forwarder na tym samym ho�cie (localhost), wygenerowanie certyfikatu odbywa si� za pomoc� polecenia:

```
$: cd /etc/pki/tls/
$: openssl req -x509 -newkey rsa:2048 -keyout private/logstash-forwarder.key -out certs/logstash-forwarder.crt -nodes -days 3650
```

Wa�ne aby nast�puj�ce dane uzupe�ni� w nast�puj�cy spos�b:

```
Common name: localhost
Email address: root@localhost
```

Nast�pnie trzeba zamapowa� lokalizacj� certyfikat�w. Przed uruchomieniem ```docker-compose up``` nale�y zmodyfikowa� odrobin� plik *docker-compose.yml*:

```
logstash:
  (...)
  volumes:
    - /etc/pki/tls:/etc/pki/tls  # t� linijk� nale�y dopisa�
    - ./logstash/config:/etc/logstash/conf.d
```

Rozwi�zanie zaczerpni�te z: https://github.com/cityindex-attic/logsearch/wiki/Lumberjack-Locally)

##### Other host

Na serwerze *ELK*, nale�y wygenerowa� certyfikat:

```
$: cd /etc/pki/tls/

$: openssl req -x509 -batch -nodes -newkey rsa:2048  -days 365 -keyout private/logstash-forwarder.key -out certs/logstash-forwarder.crt

$: openssl req -x509  -batch -nodes -newkey rsa:2048 -days 365 -keyout private/logstash-forwarder.key -out certs/logstash-forwarder.crt -subj /CN=logstash.example.com
```

Nast�pnie plik *logstash-forwarder.crt* b�dzie tak�e wykorzystywany na *Linux Node*. Najlepiej niech znajduje si� w takiej samej lokalizacji. Korzysta� b�dzie z niego *logastash-forwarder*.

#### Docker ELK Stack

Najprostrzym sposobem na rozpocz�cie przygody z *ELK Stack* jest uruchomienie gotowego kontenera ([Docker-Elk](https://github.com/deviantony/docker-elk)):

```
$: git clone git@github.com:deviantony/docker-elk.git
$: cd docker-elk
$: docker-compose up -d
```

Po prawid�owym uruchomieniu na przedstawionych portach zostaj� uruchomione nast�puj�ce us�ugi:

- *5000*: Logstash TCP input
- *9200*: Elasticsearch HTTP
- *5601*: Kibana Web Interface

Teraz wystarczy tylko otworzy� adres [http://localhost:5061](http://localhost:5061) w przegl�darce aby uzyska� dost�p do interfejsu Kibana.

#### Logstash

https://www.elastic.co/downloads/logstash
https://www.elastic.co/guide/en/logstash/current/configuration-file-structure.html

### LAMP Node

#### Logstash Forwarder

syslog, auth.log,
ftp? ssh?

php logs:
- http://stackoverflow.com/questions/5127838/where-does-php-store-the-error-log-php5-apache-fastcgi-cpanel
- http://askubuntu.com/questions/14763/where-are-the-apache-and-php-log-files

app logs

```
$: sudo vi /etc/logstash-forwarder.conf
```

```json
{
  "network": {
    "servers": ["localhost:5000"],
    "timeout": 15,
    "ssl ca": "/etc/pki/tls/certs/logstash-forwarder.crt"
  },
  "files": [{
    "paths": [
      "/var/log/syslog",
      "/var/log/auth.log"
    ],
    "fields": {"type": "syslog"}
  }]
}
```

```
$: sudo service logstash-forwarder restart
```

## ELK a skalowalno��

![scalable-architecture.png](scalable-architecture.png)

## Przydatne linki

- [Elastic](http://elastic.co)
- [Elastic GitHub](https://github.com/elastic)
- [Grok Debug](http://grokdebug.herokuapp.com)
- [Grok Patterns](https://github.com/logstash-plugins/logstash-patterns-core/tree/master/patterns)
- [Scaling an ELK stack](http://www.slideshare.net/renzotoma39/scaling-an-elk-stack-at-bolcom-39412550)