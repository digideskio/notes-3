# Big Data - Apache Hadoop

## Hadoop

- rozproszony system do przetwarzania informacji
- korzysta z paradygmatu MapReduce
- fault-tolerant
- �atwa skalowalno�� - szybkie dok�adanie kolejnego w�z�a Hadoop
- projekt Open Source
- Hadoop Core to:
  - HDFS
  - YARN
  - MapReduce
  - Hadoop Common

## HDFS

- rozproszony system plik�w
- nie wymaga specjalnego sprz�tu - uruchamiany na przeci�tnych maszynach
- strumieniowy dost�p do danych
- skoncentrowany na obs�ug� du�ych plik�w

## MapReduce

### Trzy etapy MapReduce

- Map - przetwarzanie danych wej�ciowych do postaci ```klucz : warto��```
- Shuffle - sortowanie i grupowanie danych wzgl�dem kluczy
- Reduce - ��czenie danych, przygotowanie ostatecznego wyniku

```
Input -> Map -> Shuffle -> Reduce
```

### Przyk�ady:

- [Word Count Example](map-reduce-word-count-example/)
