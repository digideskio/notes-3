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
  - podobny do IPython
  - je�eli nie podamy skryptu do uruchomienia
  - z tego trybu mo�na tak�e uruchomi� skrypty za pomoc� polece� run i exec
  - uzupe�nianie kodu (tab), historia instrukcji
  - tryb nadaj�cy si� do test�w
- batch - bezpo�rednie uruchamianie skrypt�w
  - ```pig scriptname.pig```
- PigServer
  - uruchamianie skrypt�w z poziomu kodu Javy (klasy: PigServer, PigRunner)
  
## Pig Latin

### LOAD

Syntax: ```LOAD 'data' [USING function] [AS schema];```

Wczytywanie danych, wynikiem jest relacja (zestaw krotek, krotka = wiersz danych).

*Za�adowanie pliku, bez schematu (```DESCRIBE text; ``` zwr�ci "Schema for text unknown."):*

```pig
text = LOAD 'wordcount.txt';
```

*Za�adowanie pliku z ustawieniem schematu (ka�dy wiersz pliku jest tablic� znak�w w formacie UTF-16):*

```pig
text = LOAD 'wordcount.txt' AS (line:chararray);
```

*Za�adowanie pliku z ustawieniem separatora:*

```pig
stock = LOAD 'stock.csv' using PigStorage(',') AS (product:chararray, price:float, category:chararray);
```

### STORE

Syntax: ```STORE alias INTO 'directory' [USING function];```

Zapisanie danych w okre�lonym formacie do wskazanego folderu.

```pig
STORE wordcount INTO 'wordcount-result' USING PigStorage(',');
```

### DESCRIBE

Schemat relacji.

```pig
DESCRIBE stock;

-- stock: {product: chararray,price: float,category: chararray}
```

### DUMP

Zrzut relacji.

```pig
DUMP stock;

-- (product1,100.0,category1)
-- (product2,200.11,category1)
-- (product3,245.44,category2)
-- (product1,100.0,category1)
-- (product2,200.11,category1)
```

### FOREACH

Modyfikacja p�l w relacji, przekszta�canie relacji.

```pig
text = LOAD 'wordcount.txt' AS (line:chararray);
chars_per_line = FOREACH text GENERATE SIZE(line) AS chars;

DUMP chars_per_line;

-- (16)
-- (48)
-- (89)
-- (81)
```

### TOKENIZE

Rozbija tablic� znak�w na zbi�r s��w.

```pig
text = LOAD 'wordcount.txt' AS (line:chararray);
words = FOREACH text GENERATE TOKENIZE(line) AS word;

DUMP words;

-- ({(Lorem),(ipsum),(dolor)})
-- ({(Next),(line),(with),(text)})
```

### FLATTEN

Sp�aczenie struktury. Usuwa zagnie�d�enia ze zbior�w i krotek.

```pig
text = LOAD 'wordcount.txt' AS (line:chararray);
words = FOREACH text GENERATE FLATTEN(TOKENIZE(line)) AS word;

DUMP words;

-- (Lorem)
-- (ipsum)
-- (dolor)
-- (Next)
-- (line)
-- (with)
-- (text)
```

### GROUP

Grupowanie krotek w relacji wzgl�dem wskazanego pola.

```pig
text = LOAD 'wordcount.txt' AS (line:chararray);
words = FOREACH text GENERATE FLATTEN(TOKENIZE(line)) AS word;
grouped = GROUP words BY word;

DUMP grouped;

-- (the,{(the),(the),(the)})
```

### ORDER

Sortuj krotki w relacji wzg�dem wskazanego pola i kolejno�ci.

```pig
sorted = ORDER counts BY total DESC;
```

### LIMIT

Limituj relacj� do wskazanej ilo��i krotek.

```pig
limited = LIMIT sorted 10;
```

### EXPLAIN

Plan logiczny, fizyczny oraz zadania w modelu MapReduce.

```pig
EXPLAIN stock;
```

*Logical Plan:*

```
stock: (Name: LOStore Schema: product#13:chararray,price#14:float,category#15:chararray)
|
|---stock: (Name: LOForEach Schema: product#13:chararray,price#14:float,category#15:chararray)
    |   |
    |   (Name: LOGenerate[false,false,false] Schema: product#13:chararray,price#14:float,category#15:chararray)ColumnPrune:InputUids=[13, 14, 15]ColumnPrune:OutputUids=[13, 14, 15]
    |   |   |
    |   |   (Name: Cast Type: chararray Uid: 13)
    |   |   |---product:(Name: Project Type: bytearray Uid: 13 Input: 0 Column: (*))
    |   |   (Name: Cast Type: float Uid: 14)
    |   |   |---price:(Name: Project Type: bytearray Uid: 14 Input: 1 Column: (*))
    |   |   (Name: Cast Type: chararray Uid: 15)
    |   |   |---category:(Name: Project Type: bytearray Uid: 15 Input: 2 Column: (*))
    |   |
    |   |---(Name: LOInnerLoad[0] Schema: product#13:bytearray)
    |   |---(Name: LOInnerLoad[1] Schema: price#14:bytearray)
    |   |---(Name: LOInnerLoad[2] Schema: category#15:bytearray)
    |
    |---stock: (Name: LOLoad Schema: product#13:bytearray,price#14:bytearray,category#15:bytearray)RequiredFields:null
```

*Physical Plan:*

```
stock: Store(fakefile:org.apache.pig.builtin.PigStorage) - scope-24
|
|---stock: New For Each(false,false,false)[bag] - scope-23
    |   |
    |   Cast[chararray] - scope-15
    |   |---Project[bytearray][0] - scope-14
    |   Cast[float] - scope-18
    |   |---Project[bytearray][1] - scope-17
    |   Cast[chararray] - scope-21
    |   |---Project[bytearray][2] - scope-20
    |
    |---stock: Load(file:///home/bigdata/stock.csv:PigStorage(',')) - scope-13
```

*Map Reduce Plan:*

```
MapReduce node scope-25
Map Plan
stock: Store(fakefile:org.apache.pig.builtin.PigStorage) - scope-24
|
|---stock: New For Each(false,false,false)[bag] - scope-23
    |   |
    |   Cast[chararray] - scope-15
    |   |---Project[bytearray][0] - scope-14
    |   Cast[float] - scope-18
    |   |---Project[bytearray][1] - scope-17
    |   Cast[chararray] - scope-21 
    |   |---Project[bytearray][2] - scope-20
    |
    |---stock: Load(file:///home/bigdata/stock.csv:PigStorage(',')) - scope-13--------
Global sort: false
```

## Przyk�ady:

- [Word Count Example](pig-word-count-example/)