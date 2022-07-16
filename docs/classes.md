<a href='../README.md' style='border: 1px solid gold; padding: 5px; color: gold'>← back to README.md</a>

# Class basic syntax

Podstawowa struktura klasy.

```js
class MyClass {
  // class methods
  constructor() { ... }
  method1() { ... }
  method2() { ... }
  method3() { ... }
  ...
}
```

Klasa w JS jest typu `function`. Konstrukt klasy tworzy de facto funkcję User, a metody przechowuje w User.prototype.

-   Jednak obiekt dostaje specjalny znacznik, który jest sprawdzany często. Np. class musi być stworzone przez `new`.
-   metody klasy nie są numerable więc nie można ich iterować `for...in`
-   klasa jest zawsze `use strict`

```js
class User {...}

new User() // OK
User() // Error!
```

## loosing THIS, binding methods to instances

```js
class Button {
    // może doprowadzić do utracenia połączenia z this. Wywołanie metody wpływa na this!
    click() {
        alert(this.value);
    }
}
```

Rozwiązaniem jest stworzenie referencji do funkcji

```js
class Button {
    // pole click jest tworzone per-obiect (każdy ma swoją instancję funkcji). This pozostaje zbindowane z instancją klasy.
    click = () => {
        alert(this.value);
    };
}
```

## class expression

Klas można używać tak, jak funkcji - tzn. przypisać je bezpośrednio obiektowi

```js
let User = class {
    sayHi() {
        alert('Hello');
    }
};
```

## get/set

Klasy mogą zawierać gettery i settery. Można się od nich odwoływać bezpośrednio, jak do pól właściwości.

```js
class User {
    constructor(name) {
        // invokes the setter
        this.name = name;
    }

    get name() {
        return this._name;
    }

    set name(value) {
        if (value.length < 4) {
            alert('Name is too short.');
            return;
        }
        this._name = value;
    }
}

let user = new User('John');
alert(user.name); // John

user = new User(''); // Name is too short.
```

# Class inheritance

Dziedziczenie (inheritance) jest sposobem na rozszerzanie klasy o inną.
Dodajemy funkcjonalność na bazie innej klasy.

`class Child extends Parent {...}`

## nadpisywanie metod

Możemy nadpisać metodę parenta całkowicie:

```js
class Rabbit extends Animal {
    stop() {
        // ...now this will be used for rabbit.stop()
        // instead of stop() from class Animal
    }
}
```

lub częściowo (np. dodając funkcjonalność) przez odwołanie do metody nadrzędnej `super.metchod()`

```js
  stop() {
    super.stop(); // call parent stop
    this.hide(); // and then hide
  }
```

## nadpisywanie konstruktora

Konstruktor klasy jest tworzony automatycznie.
Jeśli tworzymy własny - musimy odwołać się do konstruktora parenta (super())
Chodzi o przypisanie `this`. Przy tworzeniu konstruktora obiekt oczekuje, że tematem `this` zajmie się klasa wyższa.

```js
class Rabbit extends Animal {
    constructor(name, earLength) {
        super(name);
        // other stuff
    }
    // ...
}
```

# Static properties and methods

Metody statyczne dotyczą całej klasy. Wprowadzamy je za pomocą słowa `static` i używamy bezpośrednio. Mogą być używane jako np. metody CRUD. Są też dziedziczone przez klasy potomne.

```js
class User {
    static staticMethod() {
        alert(this === User);
    }
}

User.staticMethod(); // true
```

`this` w metodach statycznych odwołuje się do klasy.

```js
  static createTodays() {
    // remember, this = Article
    return new this("Today's digest", new Date());
  }
```

# Private and protected properties and methods (encapsulation)

Właściwości klasy mogą być prywatne lub publiczne. Nie dzieje się to na poziomie języka (brak implementacji tego), więc oznacza się pola jako prywatne przez podkreślenie `_`.

Pola takie można ustawiać setterem i pobierać getterem, celem lepszej kontroli.

```js
class CoffeeMachine {
    _waterAmount = 0; // pole oznaczone "prywatne"

    // setter pozwala na kontrolę wartości
    set waterAmount(value) {
        if (value < 0) {
            value = 0;
        }
        this._waterAmount = value;
    }

    //  getter pozwala na kontrolę pobierania
    get waterAmount() {
        return this._waterAmount;
    }

    constructor(power) {
        this._power = power;
    }
}

let coffeeMachine = new CoffeeMachine(100);
// add water
coffeeMachine.waterAmount = -10; // _waterAmount will become 0, not -10
```

## readonly values

Czasami niektóre wartości nie powinny być zmieniane w trakcie. Wtedy ustawiamy je w konstruktorze i nie dajemy settera, tylko getter.

```js
class CoffeeMachine {
  // ...

  constructor(power) {
    this._power = power; // ustalamy wartość wyjściową
  }

  get power() {
    return this._power;
  }
  // ... nie dajemy settera

}

let coffeeMachine = new CoffeeMachine(100);
alert(`Power is: ${coffeeMachine.power}W`); // Power is: 100W
coffeeMachine.power = 25; // Error (no setter)
```

## gettery, settery

Gettery, settery są wygodne, stosuje się bezpośrednio (jak pola), jednak metody mają tę przewagę, że przyjmują argumenty.

## pola prywatne `#`

Propozycja rozwoju języka JS poprzez dodanie pól prywatnych dla klasy za pomocą `#polePrywatne`. Jeszcze nie wprowadzona?


# Extending built-in classes

Klasy wbudowane również można rozszerzać. 

Uwaga: normalnie klasy dziedziczą metody statyczne. Jednak **klasy wbudowane nie dziedziczą metod statycznych od swoich rodziców!!!**. Tylko prototyp dziedziczy od prototypu rodzica.

Instanceof sprawdza łańcuch prototypu przy badaniu dziedziczności.

# Class checking: "instanceof"

`instanceof` operator pozwala na sprawdzenie klasy obiektu. Działa także na klasy dziedziczone i potomne.

```obj instanceof Class```


# Mixins

Z założenia można rozszerzać tylko jedną klasę (diamond problem). Jednak można kopiować metody do prototypu klasy (jako ułomne dziedziczenie metod)

Więcej... https://javascript.info/mixins

<a href='../README.md' style='border: 1px solid gold; padding: 5px; color: gold'>← back to README.md</a>
<a href='#top' style='border: 1px solid gold; padding: 5px; color: gold'>↑ back to top</a>
