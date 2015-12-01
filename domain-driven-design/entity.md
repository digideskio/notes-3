# Domain-Driven Design - Entity

## Notatki

Reprezentacja byt�w posiadaj�cych to�samo�ci - unikalny identyfikator.

## Przyk�ad

```php
namespace BusinessName/Domain/Entity;

class User
{
    // ...
    
    public function getFullName()
    {
        return $this->firstName . ' ' . $this->lastName;
    }
}
```