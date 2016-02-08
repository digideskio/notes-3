# C# - Notes

Notatki dotycz�ce j�zyka C#

## Accessors

```cs
class Person {
    public string FirstName { get; set; }
    
    private string lastname;
    public string LastName {
        get {
            return lastname;
        }
        set {
            lastname = value;
        }
    }
    
    private double age;
    public double Age {
        private set {
            age = Math.Ceiling(value / 12);
        }
        get {
            return age;
        }
    }
}
```

- *get* wywo�ywany w momencie pobierania warto�� w�a�ciwo�ci
- *set* wywo�ywany w momencie nadania warto�ci w�a�ciwo�ci
- s�owo kluczowe *value* - reprezentacja warto�ci przypisywanej do w�a�ciwo�ci w akcesorze set