# PHP - SPL - Functions

## class_implements()

Zwraca list� interfejs�w, kt�re zosta�y zaimplementowane przez klas� lub obiekt.

```php
interface Foo {}
class Bar implements Foo {}

var_dump(class_implements(new Bar));

/* Result:
array(1) {
  'Foo' =>
  string(3) "Foo"
}
*/
```

## iterator_to_array()

Konwertuje interator na tablic�.

```php
$it = new ArrayIterator(['name' => 'Lorem Ipsum', 'Foo', 'Bar']);

var_dump(iterator_to_array($it));

/* Result:
array(3) {
  'name' =>
  string(11) "Lorem Ipsum"
  [0] =>
  string(3) "Foo"
  [1] =>
  string(3) "Bar"
}
*/
```

## spl_autoload_register()

Umo�liwia rejestrowanie funkcji (jednej i wi�cej) kt�re obs�uguj� automatyczne do��czanie plik�w klas w momencie gdy tworzymy obiekt danej klasy. Funkcje �aduj�ce b�d� wykonywane w kolejno�ci w jakiej zosta�y zarejestrowane, tak d�ugo a� zostanie za�adowana ��dana klasa.

```php
spl_autoload_register(function ($class) {
    include 'classes/' . $class . '.class.php';
});
```

## spl_object_hash()

Zwraca unikalny identyfikator obiektu, kt�rego mo�na u�ywa� do identyfikacji obiektu.

```php
$object1 = new stdClass;
$object1->name = 'Foo Bar';
$object1->age = 20;

var_dump(md5(spl_object_hash($object1)));

$object2 = clone $object1;

var_dump(md5(spl_object_hash($object2)));

/* Result:
string(32) "3b4ac85bf676cb8b44cb87fc1fd41c66"
string(32) "e0f906c44e99a93d4e01d6f5f41a9ec2"
*/
```