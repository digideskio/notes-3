# International PHP Conference 2015 Spring - Berlin

Data: *7 - 11 czerwca 2015*

Trendy: *Unit Testing*, *Domain-driven Development*, *Test-driven Development*, *Good Practice - SOLID*.

## Test-Driven Domain

warsztat: Sebastian Bergmann, Stefan Priebsch � thephp.cc

- Bizness zale�ny od technologii - �LE - w dupie masz z jakiego skorzysztasz framerowka/liba ? Abstrakcja. Nieuzale�nianie si� od technologii.
- Migracje zf1->sf2 (rewrite ca�ego kodu), a powinismy zrobi� write adapters do bissnes logic
- UI -> Bussines <- Infrastructure (odcinamy si� od tego: framework, persistance � database)
- Analogia do piwa, sk�d jest (czy z baru, sklepu, etc) � nikogo z nas to nie interesuje, wa�ne ze je mamy na wyciagni�cie reki i mo�emy ugasic pragnienie. Bar � to API, implementacja interfejsu
- Bussines opary o interface � wa�ne co chcemy, nie wa�ne w jaki spos�b zostanie to zrealizowane ! (php 5.x w tym miejscu sssie, bo nie mo�esz okreslic ze oczekujesz kolekcji obiekt�w Y)
- Interfejsy i adaptery, Dependency inversion, Inversion of Control
- Co z libami zewn�trznymi od kt�rych uzale�niony jest nasz BL? Np. new Data(), new Moment(). Podobnie jak z FV � czyli kolejny adapter
- �DDD� w PHPunit, bardziej opisowe testy, opisuj�ce regu�y biznesu (np. PremiumContract: - runs for twelve months, - can be renewed at any time, - cannot be downgraded to standard contract), uruchomiony z testdox parameter, output moze posluzyc wtedy juz jako opis regul biznesu na wiki
- w takim wypadku testy jednostkowe nie s� tylko dokumentacj� dla programisty ale tak�e dla biznesu
- phpab � PHP Autoload Builder
- PHPUnit - @depends tag
- Prezentacja, programming w zakresie � DDD w TDD. Wyt�umaczenie koncepcji wzorca Repository.
- Phary � composer/phpunit � nie w composer.json i si�ganie per build a dost�pne globalnie lub w folderze build/bin. Narz�dzie sk�adowe projektu wrzucone w CVS.
- Kasa zawsze w intigerach (groszach, z� / 100 gr).  new Money(100, �PLN�)
- Daty nie timestapy! � DateTime Objecty z Timezonem
- Uniezale�nia� kod od time() zahardkowanego w kodzie, zawsze przekazywa� jako parametr
- CR na koniec nie dotyczy� TDD/DDD a ma�ych niewa�nych g�wien w stylu: startDate, endDate -> dateRange(s, e), brak modyfikatora dost�pu przed function etc.
- Identyfikuj najwa�niejsze elementy logiki biznesowej � to pierwsze co powiniene� implementowa�. Zamodeluj najwa�niejsze Entity

Powi�zane tematy to tak�e: Hexagonal, Onion � Architecture, CQRS

## Composer Best Practices

- Om�wienie semantic versioning
- Przedstawienie �script� w Composerze. Aby zapisa� skr�ty do komend, np. potem tylko �composer test� i uruchamiaj� si� testy aplikacji
- Commitowanie composer.lock (nie zgadzam si�). Aby by�a powtarzalna instalacja u ka�dego. Pami�ta� o nie deployowaniu go na serwer produkcyjny � mo�na to potem wyszuka� via gogle
- Om�wienie problemu konfliktu zale�no�ci � vendor 1 korzystac z pack1.0 a vendor 2 korzysta z pack2.0 � problem nie do rozwi�zania

## Cloud Development with a service of SasS � A Stroy to Continous Improvement

- Azure � rozwi�zanie na wszystko � nawet development kt�ry powinien odbywa� si� w Cloudzie :stuck_out_tongue_winking_eye:
- Vagrant, Wirtualne maszyny to z�o
- Przej�cie w Clouda mo�e wi�za� si� ze zmian� modelu biznesowego � np. Przechodzimy bo b�dziemy robi� z naszej apki SasS

## Five weird Tricks to become a better Developer

- My�l
- B�d� empatyczny
- Masz do�a, brak Ci motywacji � odetnij si� na chwile od developmentu, zr�b sobie przerw�, zajmij si� czym� innym
- B�d� pragmatyczny
- Po�wi�� czasem wi�cej czasu, zautomatyzuj proces, u�ywaj m�drze narz�dzi, czytaj komunikaty � aby� nie musia� co uruchomienie softu klika� �zamknij� w okienku z tipami, po prostu je wy��cz

## Deep Dive into Browser Performance

- Performance przegl�darki jest bardzo wa�ny, to jest to co widzi u�ytkownik � czyli to co na niego jest najwa�niejsze
- Minimalizowa� lookupy DNS
- Optymalizacja CSS�w � zmiejszanie ilo�ci selektor�w CSS, usuwanie nieu�ywanych
- JS � minifikacja, konkatenacja, standard
- Gzipowanie outputu
- Minimalizacja repaint�w (to o czym gadali tak�e koledzy z Ukrainy)
- Nie u�ywa� XPath
- Profilowa�, profilowa�, profilowa� � nie tylko lokalnie

## Modernize your PHP Codes

- Przedstawienie nowych featur�w od PHP 5.3 do 5.6 (namespaces, traits � generalnie g��wne zmiany)

## Profiling with XHProof

- Om�wienie czym jest, �e ma mo�liwo�� zrzucania danych do � MySQL, MongoDb, Przedstawi� narz�dzie do wy�wietlania log�w (te standardowe)
- Nie profiluj tylko lokalnie, profiluj te� zdalnie � co kt�ry� request, na rzeczywistych danych
- Pami�taj o czyszczeniu log�w z xhproofa (aby Twoja baza nie wa�y�a potem 2.5TB)
- Im wi�cej u�ytkownik�w tym zmiejszaj ilo�� request�w kt�re zapisujesz

## The Future of the Internet

Przedstawienie r�nych wizji przysz�o�ci - np wszczepiane chipy, wyostrzanie zmys��w, zgrywanie �wiadomo�ci, AI jako osobisty asystent cz�owieka, VR, AR.

## ECMAScript 6 for real

- wykorzystywanie ECMAScript 6 w projektach produkcyjnych, pomimo braku wsparcia w przegl�darkach
- stack technologiczny i flow - co jest niezb�dne do testowania i uruchomienia

## The three Dimensions of Testing

Bergmann przedstawi� podzia� test�w na 3 wymiary:

 - rola (np edge to edge testing) - nale�y zawsze okre�li� sobie po co chcemy wykonywa� testy, 
 - scope (ile cz�ci aplikacji potrzebne jest do uruchomienia test�w), im mniej tym lepiej bo testy s� dok�adniejsze,
 - implementacja (narz�dzia i spos�b pisania test�w np phpunit + webdriver). 
 
Zwr�ci� uwag� na potrzeb� robienia test�w integracyjnych - poda� przyk�ad rozbitej sondy marsja�skiej przez barak test�w integracyjnych. 2 teamy kodzi�y modu� GPS obliczaj�cy trajektori� wej�cia w atmosfer� marsa. Jedni zakodzili w jednostkach metrycznych a drudzy w imperialnych. Dlatego trajektoria zosta�a �le obliczona i sonda si� rozbi�a. Testy powinny by� bardzo wra�liwe na nieprawid�owe zachowania kodu i powinny jasno wskazywa� miejsce w kt�rym jest co� nie tak. Kod testowy r�wnie� powinien by� pisany zgodnie z regu�ami clean code. Na koniec pojazd po phpspec - wg Bergmana jest tam za du�o magii. 

Jak obiekty gadaj� ze sob� przez mockowanie:
po co si� mockuje, Rodzaje mock�w, solid - jak dzi�ki testom wy�apa� b��dy zwi�zane z SOLID, nowy tool do mockowania w phpunit - prophecy, 
http://www.slideshare.net/everzet/design-how-your-objects-talk-through-mocking,

## Take care of your Logs with ELK

- Elasticsearch, Logstash, Kibana.
- oraz narz�dzia poboczne curator, heka, rsyslog, logstash-forwarder, logtail, monolog 
- aplikacje wspomagaj�ce logowanie, pokaza� konfiguracj� i jak to w rzeczywisto�ci wygl�da

## Extract till you drop

Refactor Legacy Code na �ywo - wykorzstanie mo�liwo�ci PHPStorm (skr�ty klawiszowe) z PHPUnit i CodeCoverage

## Localize your Frontend

Zaadresowano g��wne problemy zwi�zne z lokalizacj� frontendu. Formatowanie liczb, dat, czasu, pieni�dzy, string�w. M�wi�a o narz�dziu wspmogaj�cym lokalizowanie frontu - phraseup. M�wi�a o bibliotekach numeric.js, google closure, max ci powie, intl w przegl�darce - koncept na razie nie rozwini�ty.

## Decoupling the Model from the Framework

Gadka o Event driven design. Na pocz�tku nale�y sobie wylistowa� jakie zdarzenia domenowe maj� miejsce w naszym systemie, a nast�pnie zamodelowa�sobie ca�y flow propagacji event�w i ich obs�ugi.

## PHP 7: What changed internally?

Mi�so - w jaki spos�b zoptymalizowano core nowego php �e uzyskano 2 krotny wzrost wydajno�ci. Go�� om�wi� r�nice (php 5.5 vs php 7) w strukturach danych typ�w danych w php 

## Surviving the next Upgrade

Stefan opowiedzia� o problemie zale�no�ci w projektach. Wskaza� problem aktualizowania vendora kt�rego wykorzystujemy w projekcie. Nie powinno by� sytu�acji kiedy nasz biznes zale�y od kogo� innego i nie mamy nad tym tak naprawd� �adnej kontroli. Pokaza� jak do tego dobrze podchodzi� (najlepiej unika�). Przekazywanie ca�ego kontenera DI np do kontrollera nie jest dobrym pomys�em. Poda� przyk�ad z symfony kod $this->get('mailer'); nie wiadomo co zwraca. Lepiej zrobi� metod� getMailer() gdzie mo�na zdefiniowa� sobie konkretnie co metoda ma zwr�ci�, a najlepiej wstrzykiwa� zale�no��przez konstruktor. Zawsze nale�y konkretnie definiowa� zale�no�ci. Aby odzieli� nasz� domen� od frameworka nale�y definiowa� interfejs kt�ry b�dzie odzwierciedla� potrzeby logiki domenowej a nast�pnie pisa� do tego adaptery. Dzi�ki temu zmiana w zale�nych komponentach frameworka poci�gnie za sob� jedynie konieczno�� zmiany adaptera a nie interfejsu naszej domeny. 

## Optimizing the Performance of Mobile Web Apps

Og�lnie head of development mobidev wywar� bardzo s�abe wra�enie. Panowie pokazali kilka przydatnych tip�w kt�re mo�na wykorzysta� podczas tworzenia aplikacji mobilnej. Pokazali flow jak wygl�da generowanie strony html w przegl�darce. Pokazali problem z repaintem element�w layoutu podczas robienia animacji. Pokazali jak przenosi� operacje do gpu kt�re jesty wydajniejsze przy niekt�rych animacjach. Wykorzystanie wzorc�w projektowych Flyweight i Observer.