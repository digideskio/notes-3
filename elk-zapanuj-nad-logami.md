# ELK - Zapanuj Nad Logami

ELK - Elasticsearch Logstash Kibana

## Spis tre�ci

1) Logi
2) ELK
3) Praktyka

## Logi

Czym s� logi? Po co je sstosujemy? Problemy z logami.

- "(...) zapis zawieraj�cy informacj� o zdarzeniach i dzia�aniach dotycz�cych systemu informatycznego, systemu komputerowego czy komputera." via Wikipedia.
- umo�liwiaj� analiz� dzia�ania systemu, detekcj�:
   - b��d�w, nieprawid�owo�ci dzia�ania,
   - pr�b i sposob�w w�ama�
- multum log�w, w zale�no�ci od u�ytego stacku technologicznego. Prosta web aplikacja to logi z:
   - Linux Logs (Syslog, Auth Log, FTPD Log, etc.)
   - Apache,
   - PHP,
   - MySQL (slowlog, https://dev.mysql.com/doc/refman/5.5/en/server-logs.html),
   - Application Logs.
- ci�ko si� je przegl�da (Old School way : SSH -> tail, cat, grep, less, awk ...)
- du�y rozmiar - powodzenia przy analizie 100GB nieustrukturyzowanych log�w
- nale�y wprowadzi� polityka archiwizacji log�w
- niepotrzebne - z punktu widzenia klienta, nie przynosz� korzy�ci biznesowej a jednak:
  - s�...,
  - developerzy kodz� ```$log->warning('...');```
- nie przegl�damy ich - kto w ci�gu ostatniego miesi�ca sam z siebie rzuci� okiem na logi?

## ELK

*Elasticsearch + Logstash + Kibana*

- trzy niezale�ne od siebie open-sourceowe projekty
- vendor - firma Elastic
- "How can you maintain blazing-fast analytics as you data grows larger and larger? Answer: the ELK stack makes it way easier -- and way faster -- to search and analyze large data sets."

// Grafika jak to dzia�a

// ze logstash zbiera, elastic przechowyje (DB) kinbana korzysta z DB i wy�wietla u�ytkownikowi

### Elasticsearch

Indexing, storage and retrieval engine

### Logstash

Log input slicer and dicer and output writer

- https://github.com/elastic/logstash/tree/v1.4.2/patterns

### Kibana

Data Displayer

## Praktyka

Opis dotyczy uruchomienia serwera *ELK* oraz konfiguracji serwer�w aplikacyjnych (*LAMP Node* - Linux, Apache, MySQL, PHP) kt�re za pomoc� *Logstash Forwarder* b�d� przekazywa� logi do serwera *ELK*.

// grafika prezentuj�ca architektur� serwer�w

### ELK Server

### LAMP Node

Predefined requirements dla serwera *ELK*:

- Git Client
- Docker & Docker Compose

### Wygenerowanie certyfikatu SSL

Rekomendowana lokalizacja plik�w:

- certificates (.crt): ```/etc/pki/tls/certs/```
- keys (.key): ```/etc/pki/tls/private/```

Wygenerowane certyfikaty b�d� potrzebne dla *logstash* (pliki .crt oraz .key) i *logstash-forwarder* (tylko plik .crt).

#### Localhost

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

#### Other host

Na serwerze *ELK*, nale�y wygenerowa� certyfikat:

```
$: cd /etc/pki/tls/

$: openssl req -x509 -batch -nodes -newkey rsa:2048  -days 365 -keyout private/logstash-forwarder.key -out certs/logstash-forwarder.crt

$: openssl req -x509  -batch -nodes -newkey rsa:2048 -days 365 -keyout private/logstash-forwarder.key -out certs/logstash-forwarder.crt -subj /CN=logstash.example.com
```

Nast�pnie plik *logstash-forwarder.crt* b�dzie tak�e wykorzystywany na *Linux Node*. Najlepiej niech znajduje si� w takiej samej lokalizacji. Korzysta� b�dzie z niego *logastash-forwarder*.


### Docker ELK Stack

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

### Logstash

https://www.elastic.co/downloads/logstash
https://www.elastic.co/guide/en/logstash/current/configuration-file-structure.html

### Logstash Forwarder

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

## Notatki

Pami�ta� o:
- na jakich licencjach oparty jest ELK
- alternatywa dla ELK
- jak wygl�da wdro�enie ELK do dzia�aj�cego �rodowiska?

Inne: 
- http://www.logstash.net/docs/1.4.2/tutorials/10-minute-walkthrough/
- http://www.slideshare.net/3camp/logstash-28920758
- http://www.slideshare.net/CloudElements/logstash-33327736

Logstash Forwarder:
- https://github.com/elastic/logstash-forwarder
- https://www.digitalocean.com/community/tutorials/adding-logstash-filters-to-improve-centralized-logging
- https://www.digitalocean.com/community/tutorials/how-to-install-elasticsearch-logstash-and-kibana-4-on-ubuntu-14-04
- https://wiki.zimbra.com/wiki/Centralized_Logs_-_Elasticsearch,_Logstash_and_Kibana

Prezentacje:
- http://moquet.net/talks/ipc-2015-elk/
- http://www.slideshare.net/jillesvangurp/elk-stack-34785684
- http://linuxfestnorthwest.org/sites/default/files/slides/Log%20Analysis%20with%20the%20ELK%20Stack.pdf (Grok Debuger do tworzenia filtr�w logstash)