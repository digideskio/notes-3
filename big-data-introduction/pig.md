# Big Data - Apache Pig

- platforma wspomagaj�ca analiz� du�ych zbior�w danych
- sk�ada si� z dw�ch element�w:
  - skryptowy j�zyk Pig Latin do zapisywania przep�ywu danych,
  - �rodowiska uruchomieniowe - lokalne (JVM), rozproszone
- udost�pnia wysokopoziomowy j�zyk Pig Latin umo�liwiaj�cy przeprowadzanie operacji - przekszta�cania danych wej�ciowych w wyj�ciowe
- wszystko co napisane w Pig Latin i tak jest przekszta�cane do modelu MapReduce - transparentnie dla programisty
- posiada tryb interaktywny u�atwiaj�cy debugowanie skryptu przed jego uruchomieniem dla pe�nego zbioru danych

## Tryby wykonywania

### Local Mode

- dzia�a na maszynie JVM
- korzysta z lokalnego systemu plik�w
- dobry dla ma�ych plik�w - testowania

```
# interactive mode
$: pig -x local

# batch mode
$: pig -x local scriptname.pig
```

### MapReduce Mode

- przekszta�ca skrypt zapisany w j�zyku Pig Latin do modelu MapReduce
- uruchamia je w klastrze opartym o Apache Hadoop
- wersja Apache Pig musi by� zgodna z wersj� Apache Hadoop
- nalezy ustawi� konfiguracj� w pliku conf/pig.properties (w katalogu instalacji Apache Pig)

```
fs.defaultFs:hdfs://localhost/
mapreduce.framework.name=yarn
yarn.resourcemanager.address=localhost:8032
```

Tryb MapReduce jest domy�lny. Uruchomienie Apache Pig w tym trybie nie wymaga dodatkowych argument�w.

```
$: pig

$: pig -x mapreduce
```

## Uruchamianie

- interactive - pow�oka Grunt
  - je�eli nie podamy skryptu do uruchomienia
  - z tego trybu mo�na tak�e uruchomi� skrypty za pomoc� polece� run i exec
  - uzupe�nianie kodu (tab), historia instrukcji
  - tryb nadaj�cy si� do test�w
- batch - bezpo�rednie uruchamianie skrypt�w
  - ```pig scriptname.pig```
- PigServer
  - uruchamianie skrypt�w z poziomu kodu Javy (klasy: PigServer, PigRunner)
