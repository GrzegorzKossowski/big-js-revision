<a href='../README.md' style='border: 1px solid gold; padding: 5px; color: gold'>← back to README.md</a>

# Proxy and Reflect

Proxy to wrapper na innym obiekcie, który przechwytuje operacje i obsługuje je sam lub przepuszcza nic nie robiąc.

`let proxy = new Proxy(target, handler)`

target - obiekt {}

handler - konfiguracja {}

Jeśli w `handler` jest taka sama operacja (nazwa), jak w targecie, to proxy ją wykonuje, inaczej przepuszcza wersję z `target`

Większoć pułapek (traps) w proxy dotyczy operacji read/write, jak np. get, set.
Muszą być zgodne ze specyfikacją, tzn. zwracać odpowiednie wartości.

Uwaga: Proxy powinien nadpisać target, aby nie było dostępu do starej wersji

```js
let dictionary = {
  'Hello': 'Hola',
  'Bye': 'Adiós'
};
dictionary = new Proxy(dictionary, ...); // nadpisanie przez proxy
```
more in https://javascript.info/proxy

# Eval: run a code string

Wbudowana funkcja `eval` pozwala na egzekucję kodu przedstawionego jako string. Rzadko używana.

`let result = eval(code);`

Wynikiem działania `eval` jest ostatnia linia tego kodu. `Eval` działa w leksykalnym otoczeniu, tzn. ma dostęp do zmiennych ze swojego scope'u. Może też je zmieniać.

Jednak w 'strict mode' ma własny scope wewnętrzny, więc jej zmienne, funkcje nie są widoczne na zewnątrz w otoczeniu.

# Currying

Zaawansowana technika przekształcania funkcji z przyjmującej wiele argumentów na funkcję wywoływaną wielokrotnie z tymi argumentami.

`foo(a,b,c) => foo(a)(b)(c)`

## Zastosowanie
pozwala np. na przygotowanie wstępnych wyspecjalizowanych podfunkcji, które przyjęły pierwszy argument, a następnie ich podwersji z kolejnymi argumentami.


```js
function curry(f) { // curry(f) does the currying transform
  return function(a) {
    return function(b) {
      return f(a, b);
    };
  };
}

// some function for currying
function sum(a, b) {
  return a + b;
}

// new curried function
const curriedSum = curry(sum);
curriedSum(1)(2); // 3

// new specialized function
const subSumTwo = curriedSum(2)
subSumTwo(3);   // 5
```

# Reference Type

# BigInt

BigInt - specjalny rodzaj typu do obsługi dużych liczb.

Tworzony za pomocą końcówki n lub construktora BigInt()

```js
const bigint = 1234567890123456789012345678901234567890n;

const sameBigint = BigInt("1234567890123456789012345678901234567890");

const bigintFromNumber = BigInt(10); // same as 10n
```

## operacje na bi

Podstawowe operacje na big int są dozwolone.
Jednak nie można mieszać typów. W przypadku dwóch typów, należy zrobić mapowanie typu.

```js
// number to bigint
x = bigint + BigInt(number);

// bigint to number
y = Number(bigint) + number;
```

BitInt nie wspiera operatora zamiany na number!

`alert( +bigint ); // error`

Porównania

```js
alert( 1 == 1n ); // true - wartości

alert( 1 === 1n ); // false - różne obiekty!
```



<a href='../README.md' style='border: 1px solid gold; padding: 5px; color: gold'>← back to README.md</a>
<a href='#top' style='border: 1px solid gold; padding: 5px; color: gold'>↑ back to top</a>
