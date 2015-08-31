# Domain-Driven Design - Aggregate

## Notatki

* Grupa obiekt�w powi�zanych ze sob� biznesowo. Razem tworz� sens
* Agregatem jest np. obiekt Faktura i lista powi�zanych obiekt�w PozycjaFaktury. W tym wypadku Faktura jest tak�e Aggregate Root
* Aggregate Root to korze� agregatu, jedyny obiekt z ca�ego agregatu, do kt�rego mog� odnosi� si� obiekty z poza tego agregatu
* Aggregate w Aggregate Root mog� by� �adowane od razy (eager loading) lub w momencie kiedy s� potrzebne (lazy loading)
* Usuwaj�c Aggregate Root usuwasz ca�y agregat
* �atwiejsza praca nad jednym mniejszym wycinkiem kodu, ni�eli nad ca�o�ci�