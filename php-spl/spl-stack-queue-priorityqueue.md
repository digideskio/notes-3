# PHP - SPL - SplStack, SplQueue, SplPriorityQueue

## SplStack (LIFO)

*Stos* - struktura danych, w kt�rej dane dok�adane s� na wierzch stosu i pobierane s� tak�e z wierzchu stosu (LIFO - Last In, First Out).

```php
$stack = new SplStack();

$stack->push('FooBar 1');
$stack->push('FooBar 2');
$stack->push('FooBar 3');
$stack->push('FooBar 4');

foreach($stack as $item) {
    echo $item . PHP_EOL;
}

/* Result:
FooBar 4
FooBar 3
FooBar 2
FooBar 1
*/
```

## SplQueue (FIFO)

*Kolejka* - struktura danych, w kt�rej nowe dane dopisywane s� na ko�cu kolejki, a pobierane s� z pocz�tku kolejki (FIFO - First In, First Out).

```php
$queue = new SplQueue();

$queue->push('FooBar 1');
$queue->push('FooBar 2');
$queue->push('FooBar 3');
$queue->push('FooBar 4');

foreach($queue as $item) {
    echo $item . PHP_EOL;
}

/* Result:
FooBar 1
FooBar 2
FooBar 3
FooBar 4
*/
```

## SplPriorityQueue

*Kolejka priorytetowa* - abstrakcyjny typ danych w kt�rym elementy zbioru posiadaj� okre�lon� relacj� porz�dku.

```php
<?php

$queue = new SplPriorityQueue();

$queue->insert('FooBar 1', 3);
$queue->insert('FooBar 2', 0);
$queue->insert('FooBar 3', 4);
$queue->insert('FooBar 4', 2);

foreach($queue as $item) {
    echo $item . PHP_EOL;
}

/* Result:
FooBar 3
FooBar 1
FooBar 4
FooBar 2
*/
```

Elementy wy�wietlane s� wzg�dem priorytetu (od najwy�szego do najni�szego) nadanego w metodzie insert().