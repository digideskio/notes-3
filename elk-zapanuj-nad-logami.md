# ELK - Zapanuj Nad Logami

Elasticsearch Logstash Kibana

## Spis tre�ci

1) Logi
2) ELK
3) Praktyka

Przedstawienie procesu grupowania log�w i wy�wietlenia ich.

## Logi

Czym s� logi? Po co je sstosujemy? Problemy z logami.

- "(...) zapis zawieraj�cy informacj� o zdarzeniach i dzia�aniach dotycz�cych systemu informatycznego, systemu komputerowego czy komputera." Wikipedia.
- umo�liwiaj� analiz� dzia�ania systemu, detekcj�:
  - b��d�w, nieprawid�owo�ci dzia�ania,
  - pr�b i sposob�w w�ama�
- multum log�w, w zale�no�ci od u�ytego stacku technologicznego. Prosta web aplikacja to logi z:
  - Syslog (Linux), 
  - Apache,
  - PHP,
  - MySQL (slowlog, https://dev.mysql.com/doc/refman/5.5/en/server-logs.html),
  - Application logs.
- ci�ko si� je przegl�da (SSH -> tail, cat, grep, awk ...)
- du�y rozmiar - powodzenia przy analizie 100GB nieustrukturyzowanych log�w
- polityka archiwizacji log�w
- niepotrzebne - z punktu widzenia klienta, nie przynosz� korzy�ci biznesowej a jednak je kodzimy
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

### Kibana

Data Displayer

## Praktyka

### Logstash

https://www.elastic.co/downloads/logstash
https://www.elastic.co/guide/en/logstash/current/configuration-file-structure.html

## Notatki

Pami�ta� o:
- na jakich licencjach oparty jest ELK
- alternatywa dla ELK
- jak wygl�da wdro�enie ELK do dzia�aj�cego �rodowiska?
- ELK Docker - https://github.com/deviantony/docker-elk

Sk�d pomys� na prezentacj�?
- IPC Berlin 2015
- Ostatnie zabawy z utrzymywaniem aplikacji na produkcji

Cytaty, fajne wtr�cenia:
- Wolimy jednak wizualizacje ni� czytanie :-)
- Aplikacje ros�y, my ci�gl� cat'owali�my coraz to wi�ksze logi.

Zako�czenie
- Dzi�ki! ;-)

Inne: 
- http://www.logstash.net/docs/1.4.2/tutorials/10-minute-walkthrough/
- http://www.slideshare.net/3camp/logstash-28920758
- http://www.slideshare.net/CloudElements/logstash-33327736

Prezentacje:
- http://moquet.net/talks/ipc-2015-elk/
- http://www.slideshare.net/jillesvangurp/elk-stack-34785684
- http://linuxfestnorthwest.org/sites/default/files/slides/Log%20Analysis%20with%20the%20ELK%20Stack.pdf (Grok Debuger do tworzenia filtr�w logstash)