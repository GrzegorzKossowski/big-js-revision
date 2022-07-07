<a href='../README.md' style='border: 1px solid gold; padding: 5px; color: gold'>← back to README.md</a>

# Introduction

JavaScript to język programowania używany do obsługi zachowania stron www

# Where To

Kod wstawiany jest na stronie za pomocą tagu `<script>`

```js
<script>
    document.getElementById("demo").innerHTML = "My First JavaScript";
</script>
```

Skrypty można umieszczać zarówno w nagłówku - headerze, jak i w `<body>` dokumentu. Najlepszym rozwiązaniem jest umieszczanie skryptów na końcu body, bo ich przetwarzanie blokuje proces.
Jeśli w headerze, to z atrybutem defer, co blokuje przetwarzanie skryptu

```js
<script src='myScript.js' defer></script>
```

Najlepszym rozwiązaniem jest trzymanie kodu w plikach zewnętrznych, co umożliwia keszowanie plików w przeglądarce i pobieranie równoległe.

# Output

Wynik działania JS można eksponować na kilka sposobów

-   Pisanie do elementu poprzez innerHTML.
-   Pisanie do dokumentu przez document.write() - to może nadpisać cały dokument!
-   Użycie alertu przez window.alert() - niestosowane. Blokuje stronę.
-   Pisanie na konsolę przez console.log() - lub -.error().
-   Drukowanie - widnow.print(), ale tylko bieżącego widoku.

```js
// podmienia zawartość elementu, np. paragrafu
document.getElementById('demo').innerHTML = 5 + 6;
// pisze bezpośrednio na ekran
document.write(5 + 6);
// popup z alertem, blokuje stronę
window.alert(5 + 6);
alert(5 + 6); // metody window można odwoływać się bezpośrednio
// klasyczny logger
console.log(`hello world`);
console.error(`sth went wrong`);
// proste drukowanie dokumentu
window.print();
```

# Variables

Zmienne są kontenerami zarezerwowanymi w pamięci na dane. Są 4 sposoby na zadeklarowanie zmiennych.

-   **var** - zmienna globalna, używana do 2015 r.
-   **let** - zmienna zmienna
-   **const** - zmienna stała po zadeklarowaniu wartości (nie dotyczy obiektów - wtedy jest stałą referencją).
-   **nothing**

```js
// deklaracja zmiennych
let x;
const y; → ERROR
// inicjalizacja
const y = 3;
```

zmienna bez wartości jest undefined

```js
let x; // undefined
```

# Let

Wprowadzone w 2015 roku
Nie może być zadeklarowana ponownie.
Musi być zadeklarowana wartość przed użyciem.
Scope dotyczy blocku → { let }

```js
let x = 2; // Allowed
{
    let x = 3; // Allowed
}
{
    let x = 4; // Allowed
}
```

Let jest hoistowane do góry, ale nie inicjalizowane, więc wywala error

Var jest hoistowane z globalnym zasięgiem, więc można zadeklarować potem

# Const

-   Wprowadzone w 2015 r.
-   Nie może być deklarowana ponownie.
-   Nie mogą być przypisane ponownie (const - stałe)
-   Dotyczą bloku → { const }

Musi być zadeklarowana i zainicjalizowana. Deklaruje się stałą referencję a nie wartość. Pozwala na zmiany w obiektach (array itp).

`const PI = 3.14159265359;`

Zmienne const są hoistowane i mogą być deklarowane po inicjalizacji, w obrębie scope’u.

# Operators

`+` dodawanie

`-` odejmowanie

`*` mnożenie

`**` potęgowanie (ES2016)

`/` dzielenie

`%` moduł

`++` wzrost o jedność

`--` spadek o jedność

Wybrane operatorny na stringach

`let x = 5 + 5;` → 10

`let y = "5" + 5;` → 55

`let z = "Hello" + 5;` → Hello5

## porównanie == vs ===

`==` - operator porównania. Dokonuje <u>konwersji</u> typów. `5 == "5"` da true (po konwersji, te same wartości)

`===` - operator jednolitości. Dokonuje <u>porównania</u> typów. `5 === "5"` da false (różne typy)

**Porównanie dwóch obiektów (new) zawsze da false - porównanie przez referencję!**

## Operatory typu

`typeof` - zwraca typ zmiennej

`instanceof` - zwraca true, jeśli obiket jest instancją typu

# Arithmetic

# Assignment

# Data Types

Typy danych decydują o ich zachowaniu.

-   number
-   string
-   boolean
-   undefined (value and type) - zmienna bez wartości

Dodanie numeru do stringa daje stringa. Jednak ewaluacja jest od lewej, więc taki przykład `let x = 16 + 4 + "Volvo";` da wynik `20Volvo` → 16+4=20

W zwiazku z tym taki `let x = "Volvo" + 16 + 4;` przykład da wynik `Volvo164`

Typowanie jest dynamiczne, co znaczy, że ta sama smienna (pudełko) może przyjąć różne typy:

`let x = 10`

`x="volvo"`

`x=true`

# Functions

Funkcje to bloki kodu przeznaczone do wykonywania określonych zadań. Wykonywane są, kiedy zostaną wywołane (to call).
Do funkcji wysyłane są **argumenty**.

`foo(...args)`

Funkcje przymują **parametry**.

```js
function foo(param1, param2) {
    return param1 * param2; // funkcja zwraca mnożenie
}
```

Funkcje mogą byc wywołane przez

-   eventy (event)
-   kod (call)
-   automatycznie (self invoked)

Funkcje, jeśli zwracają wartość, mogą być używane zamiast zmiennych.

Zmienne utworzone w funkcji są lokalne dlaniej i tylko dla niej. Nie wychodzą poza scope funkcji.

# Objects

Obiekty to zmienne zawierające wiele par typu klucz=wartość.

```js
const person = {
    firstName: 'John',
    lastName: 'Doe',
    age: 50,
    eyeColor: 'blue',
};
```

Odwołanie do wartości może nastapić przez klucz `objectName.propertyName` lub przez nawiasy `objectName['propertyName]`

## this

```js
const person = {
    firstName: 'John',
    lastName: 'Doe',
    id: 5566,
    fullName: function () {
        return this.firstName + ' ' + this.lastName;
    },
};
```

`This` odnosi się do obiektu. `This` zachowuje się dziwacznie. To, do czego się odwołuje, zależy od sposobu wywołania.

W metodzie **obiektu** odwołuje sie do tego obiektu.

Sam w sobie `this` odwołuje się do _globalnego obiektu_

W funkcji `this` odwołuje się do _globalnego obiektu_

W funkcji w 'strict mode' `this` jest _undefined_

W evencie `this` odwołuje się do obiektu, który przyjął event

Metody `call()`, `apply()`, `bind()` mogą przypisać this do dowolnego obiektu.

# Events

Eventy są wydarzeniami, które przytrafiają się elementom. JS może zareagować na takie wydarzenia.

Przykładowe eventy

-   załadowanie strony
-   kliknięcie
-   zmiana focus'a

`<button onclick="this.innerHTML = Date()">The time is?</button>`

## Event with name

```js
const event = new Event('eventName')
element.addEventListener('eventName' function(e) {}, false)

// trigger
element.dispatchEvent(event)
```

## Custom event

```js
// create custom events
const catFound = new CustomEvent('animalfound', {
    detail: {
        name: 'cat',
    },
});
const dogFound = new CustomEvent('animalfound', {
    detail: {
        name: 'dog',
    },
});

// add an appropriate event listener
obj.addEventListener('animalfound', e => console.log(e.detail.name));

// dispatch the events
obj.dispatchEvent(catFound);
obj.dispatchEvent(dogFound);

// "cat" and "dog" logged in the console
```

# Strings

Stringi służą do przechowywania tekstu. Można je tworzyć jako prymitywy `let x = "string"` (preferowane - string pool??? - sprawdzić!!!) lub jako obiekty `let x = new String('string')`

Wybrane znaki ucieczki

| znak | znaczenie            |
| ---- | -------------------- |
| `\b` | Backspace            |
| `\f` | Form Feed            |
| `\n` | New Line             |
| `\r` | Carriage Return      |
| `\t` | Horizontal Tabulator |
| `\v` | Vertical Tabulator   |

# String Methods

Prymitywy nie mają przypisanych metod (nie są klasami), ale JS przypisuje im metody i właściwości, kiedy je wykonuje.
Dzięki temu taka wartość `let x = "John Doe"` może mieć np. długość `length`.

## wycinanie

Do manipulowania stringiem są trzy metody:

`string.slice(start, end)` - zwraca nowy string w systemie tablicowym [start, end-1]. Jeżeli parametr jest ujemny, pozycja liczona jest od końca stringu. Brak parametru `end` powoduje podanie całej reszty, do końca.

```js
let str = 'Apple, Banana, Kiwi';

let part = str.slice(7, 13); /// → Banana
let part = str.slice(-12, -6); // → Banana
let part = str.slice(7); /// → Banana, Kiwi
```

`substring(start, end)` - jest podobny w działaniu do `slice()`, ale nie przyjmuje ujemnych wartości (zamienia je na 0).

```js
str.substring(7, 13); /// → Banana
str.substring(-7, 13); /// → Apple, Banana
```

`substr(start, length)` - wycina kawałek stringa. Drugi parametr wskazuje **długość** elementu zwracanego! Brak parametru wycina do końca strignu. Parametr ujemny liczy od końca stringu.

```js
let str = 'Apple, Banana, Kiwi';
let part = str.substr(7, 6); // → Banana
let part = str.substr(7); // → Banana, Kiwi
let part = str.substr(-4); // → Kiwi
```

## podmiana

`replace(match, newValue)` - podmienia pierwsze wystąpienie, zwraca **nowy** string, nie wpływa na źródło. Jest case sensitive - wielkość liter ma znaczenie.

Regexy wprowadzamy za pomocą `/.../i`. Jeśli chcemy zmienić wszystkie wystąpienia, używamy globalnych `/.../g`

```js
let text = 'Please visit Microsoft!';
// → Please visit W3Schools!
let newText = text.replace('MICROSOFT', 'W3Schools');
let newText = text.replace(/MICROSOFT/i, 'W3Schools');
let newText = text.replace(/Microsoft/g, 'W3Schools');
```

## padding (dodawanie)

`padStart(length, value)` i `padStart(length, value)` dodane nowe metody do uzupełniania stringa.

```js
let numb = 5;
let text = numb.toString();
let padded = text.padStart(4, '0'); // → 0005
let padded = text.padEnd(4, '0'); // → 5000
```

`string.split(match)` - rozbija string na array

```js
'this is | an example'.split('|'); // ["this is ", " an example"]
'this is an example'.split(' '); // ["this","is","an","example"]
'this is an example'.split(''); // ["t","h","i","s","i","s","a","n"...]
```

## inne metody

`string.trim()` - obcina spacje na końcach stringa

`string.concat()` - łączy dwa lub więcej stringów (podobnie jak +)

```js
text = 'Hello' + ' ' + 'World!';
text = 'Hello'.concat(' ', 'World!');
```

## dostęp do znaków

`string.charAt(position)` - zwraca znak na danej pozycji

`string.charCodeAt(position)` - zwraca UTF-16 code na danej pozycji

`"someText"[2]` - zwraca znak na danej pozycji → "m"

# String Search

Metody wyszukujące.

`string.indexOf(match, [startPos])` - zwraca pozycję pierwszego wystąpienia elementu (-1 jeśli brak)

`string.lastIndexOf(match, [startPos])` - zwraca pozycję ostatniego wystąpienia elementu (-1 jeśli brak)

`string.startsWith()` - ...

`string.endsWith()` - ...

`string.match(regexp)` - zwraca tablicę wystąpień lub null, jeśli nie było

```js
let text = 'The rain in SPAIN stays mainly in the plain';
text.match(/ain/gi); // → ['ain', 'ain', 'ain']
```

`string.includes(searchvalue, [start])` - zwraca true/false, jeśli występuje wyszukiwana wartość

## inne metody

`string.startsWith(searchvalue, length)`
`string.endsWith(searchvalue, length)`

# String Templates

Templaty uzyskujemy za pomocą \` (back-ticków). Pozwalają na wstawianie zmiennych przy użyciu `${}`

# Numbers

JS ma tylko jeden typ numeryczny. Numbers. Zawsze jest to 64-bitowy float. Dokładność sięga 15 miejsc dla integerów.

Floating może generowac błędy.

`0.2 + 0.1 = 0.30000000000000004`

rozwiązaniem jest mnożenie x<sup>10</sup> i dzielenie.

`let x = (0.2 * 10 + 0.1 * 10) / 10;` = 0.3

Dodanie numeru do stringa daje stringa. Jednak ewaluacja jest od lewej, więc taki przykład `let x = 16 + 4 + "Volvo";` da wynik `20Volvo` → 16+4=20

Operacje matematyczne na stringach numerycznych dają radę (następuje próba konwersji). Wyjątkiem jest operator `+`

```js
let x = '100';
let y = '10';
let z = x / y; // → 10
let z = x * y; // → 1000
let z = x - y; // → 90
let z = x + y; // → 10010 (!)
```

## NaN

Reserved word określające, że numer nie jest legalny. Jest wynikiem operacji na niedozwolonych numerach, takich jak dzielenie przez zero lub stringa nienumerycznego itp.

Uwaga: NaN jest typu number → typeof NaN → number!

```js
let x = 100 / 'Apple'; // → NaN
let x = 100 / '10'; // → 10 (poprawne!)
let z = x + NaN; // → NaN
typeof NaN; // → number
```

## inne

`+Infinity / - Infinity` - określenie nieskończoności

`0xFF` - wartości hexalne poprzedzone 0x

`number.toString(base)` - można określić bazę dla wyświetlenia numeru (defaultowa to 10). Inne to 2 - binary, 8 - octal, 16 - hexa

# Number Methods

`isFinite()` - sprawdza, czy wartość jest skończona (nie jest Infinite)

`isInteger()` - sprawdza czy wartość jest integerem

`isNaN()` - sprawdza, czy wartość jest NaN

`isSafeInteger()` Checks whether a value is a safe integer

`toExponential(x)` - zwraca string dużego numeru, zaokrąglony wg. notacji exponential -e np. 9.656000e+0

`toFixed(x)` - zwraca string numeru float, zaokrąglony do x miejsc po przecinku

`toLocaleString()` - zwraca strng z numeru wg. zasad danej lokalizacji geo

`toPrecision(x)` - zwraca float zaokrąglony lub rozciągnięty zerami do określonej liczby miejsc po przecinku

`toString()` - zamienia wartość numeryczną na string.

## inne

`Number()` - Returns a number, converted from its argument.

`parseFloat()` - Parses its argument and returns a floating point number

`parseInt()` - Parses its argument and returns an integer

`MAX_VALUE` - Returns the largest number possible in JavaScript

`MIN_VALUE` - Returns the smallest number possible in JavaScript

`POSITIVE_INFINITY` - Represents infinity (returned on overflow)

`NEGATIVE_INFINITY` - Represents negative infinity (returned on overflow)

`NaN` - Represents a "Not-a-Number" value

# Arrays

Tablice przechowują wiele zmiennych, danych, obiektów. Tablice są obiektami `typeof arr → object`.
Wyłącznie indeksowanie numeryczne! JS nie wspiera tablic indeksowanych nazwami.

```js
const cars = ['Saab', 'Volvo', 'BMW'];
```

Nie ma różnicy między `const arr = [values]` a const `arr = new Array(values)`. Dlatego używamy skróconej wersji.

Pobieranie elementów za pomocą indeksu `arr[index]`.

```js
fruits[0]; // zwraca pierwszy element
fruits[fruits.length - 1]; // zwraca ostatni element
```

Nawet,jeśli array jest const, jej elementy są zmienne: `arr[5] = "car"`

## sprawdzanie czy tablica

```js
const fruits = ['Banana', 'Orange', 'Apple'];
let type = typeof fruits; // źle, zwraca object !!!
Array.isArray(fruits); // zwraca true (ok)
fruits instanceof Array; // zwraca true (ok)
```

# Array Methods

## toString

`toString()` - zamienia tablicę w string oddzielany przecinkami

```js
['Banana', 'Orange', 'Apple', 'Mango'].toString(); // "Banana,Orange,Apple,Mango"
```

`arr.join('separator')` - zamienia tablicę w string oddzielany separatorem

```js
['Banana', 'Orange', 'Apple', 'Mango'].join(' * '); // "Banana * Orange * Apple * Mango"
```

## concat

`concat()` - łaczy dwie tablice. RETURN = NEW.

```js
array1.concat(array2, array3, ..., arrayX)
```

## pop, push

`pop` zwraca usunięty element

`push` zwraca długość nowej tablicy

```js
['Banana', 'Orange', 'Apple', 'Mango']
    .pop() // ["Banana", "Orange", "Apple"] - usuwa Mango
    [('Banana', 'Orange', 'Apple')].push('Kiwi'); // ["Banana", "Orange", "Apple", "Kiwi"] - dodaje Kiwi
```

## shift, unshift

`shift` zwraca usuniętą wartość

`unshift` zwraca nową długość tablicy

```js
['Banana', 'Orange', 'Apple', 'Mango']
    .shift() // ["Orange", "Apple", "Mango"] - usuwa Banana
    [('Banana', 'Orange', 'Apple')].unshift('Kiwi'); // ["Kiwi", "Banana", "Orange", "Apple"] - dodaje Kiwi na początku
```

Dodawanie elementów za pomocą indeksu `arr[index]` może doprowadzić do dziur `undefined`.

`delete arr[index]` usuwa wartość, pozostawiając `undefined`!

## slice

`slice()` - zwraca nową tablicę w przedziale. RETURN = NEW

## splice

`splice(index, [ileUsunac], [...jakieWstawic])` - dodaje (opcjonalnie), usuwa (opcjonalnie) elementy tablicy na wybranym indeksie

## every

`every(function)` - sprawdza true/false, czy wszystkie elementy spełniają warunek przekazany w funkcji. RETURN = BOOLEAN

# fill

`array.fill(value, start, end)` - podmienia elementy w podanym zakresie na value. ZMIENIA TABLICĘ!

```js
const fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.fill("Kiwi", 2, 4); → // ["Banana", "Orange", "Kiwi", "Kiwi"]
```

## copyWithin

`array.copyWithin(target, start, end)` - kopiuje część tablicy na inne miejsce (target), nadpisując stare dane!

```js
const fruits = ['Banana', 'Orange', 'Apple', 'Mango', 'Kiwi'];
fruits.copyWithin(2, 0); // → Banana,Orange,Banana,Orange
fruits.copyWithin(2, 0, 2); // → Banana,Orange,Banana,Orange,Kiwi,Papaya
```

## filter

`array.filter(function(currentValue, index, arr), thisValue)` - zwraca tablicę zawierającą elementy spełniające warunek z przekazanej funkcji. RETURN = NEW

## find, findIndex

`array.find(function(currentValue, index, arr),thisValue)` - zwraca pierwszy element spełniający warunek przekazany w funkcji

`array.findIndex(function(currentValue, index, arr), thisValue)` - zwraca index pierwszego elementu, który spełnia warunek

## forEach

`array.forEach(function(currentValue, index, arr), thisValue)` - wywołuje podaną metodę dla każdego elementu w tablicy. Nic nie zwraca.

## map

`map()` - tworzy nową tablicę zawierającą wyniki wywoływania podanej funkcji dla każdego elementu wywołującej tablicy.

```js
const doubles = numbers.map(function(x) {
    return x * 2;
});
```


## reduce

`reduce()` - redukuje tablicę do jednej wartości (np. liczy sumę). Zwraca wynik, nie zmienia oryginału.

```js
const numbers = [175, 50, 25];
// wynik 100
numbers.reduce((total, num) => {
    return total - num;
});
```

`reduceRight()` - Reduce the values of an array to a single value (going right-to-left)

## length

`length` - zwraca lub ustawia (przycina, wydłuża) długość tablicy.

## sort

`sort()` - sotruje tablicę alfabetycznie. Sortowanie dokładne wymaga podania funkcji. UWAGA: zmienia orginał!

```js
const points = [40, 100, 1, 5, 25, 10];
points.sort(); // → [1,10,100,25,40,5]
points.sort(function (a, b) {
    return a - b;
}); // → [1,5,10,25,40,100]
```

## some

`array.some(function(value, index, arr), this)` - zwraca `true` jeśli którykolwiek z elementów (pierwszy spełniający) spełnia warunek przesłany w metodzie i wychodzi. Zwraca `false`, jeśli żaden. RETURN = BOOLEAN

## inne

`reverse()` - Reverses the order of the elements in an array

`prototype` - Allows you to add properties and methods to an Array object

`lastIndexOf()` - Search the array for an element, starting at the end, and returns its position

`keys()` - Returns a Array Iteration Object, containing the keys of the original array

`join(separator)` - łączy elementy tablicy w string, dzielony separatorem

`Array.isArray(obj)` - sprawdza, czy obiekt jest tablicą

`indexOf(match, [startPoint])` - sprawdza index pierwszego wystąpienia elementu lub -1

`includes(match, [startPoint])` - sprawdza, czy array zawiera `match`

`from()` - tworzy tablicę z obiektu (np. rozbija string na litery)

`valueOf()` - Returns the primitive value of an array

# Array Sort

## sort

`sort()` - sotruje tablicę alfabetycznie. Sortowanie dokładne wymaga podania funkcji. UWAGA: zmienia orginał!

```js
const points = [40, 100, 1, 5, 25, 10];
points.sort(); // → [1,10,100,25,40,5]
points.sort(function (a, b) {
    return a - b;
}); // → [1,5,10,25,40,100]
```

Sortowanie (mieszanie) tablicy (randomowe)

```js
const points = [40, 100, 1, 5, 25, 10];
points.sort(function (a, b) {
    return 0.5 - Math.random();
});
```

Mieszanie tablicy obiektów (randomowe)

```js
const shuffled = arr
    .map(value => ({ value, sort: Math.random() }))
    .sort((a, b) => a.sort - b.sort)
    .map(({ value }) => value);
```

# Array Iteration

Metody iteracyjne operują na wszystkich elementach tablicy.

## forEach()

Iteruje po każdym elemencie wywołując podaną funkcję. Nie zwraca nic, nie zmienia tablicy.

`array.forEach((value, index, array) => {...})`

## map()

Tworzy nowy array wykonując podaną metodę na każdym elemencie.

`array.map((value, index, array) => {...})`

## filter()

Tworzy nową tablicę z elementów, które spełniają warunek

`arr.filter((value, index, array) => { condition })`

## reduce()

Wykonuje operację na każdym elemencie, by ostatecznie zwrócić jeden wynik.

`array.reduce((total, value, index, array) => {...})`

## every()

Sprawdza, czy wszystkie elementy spełniają warunek (wszystko, albo nic). Dowolna nieścisłość przerywa funkcję i zwraca false 

``array.every(( value, index, array) => { condition })``

## some()

Sprawdza, czy jakiś element spełniaj warunek (którykolwiek).


``array.some(( value, index, array) => { condition })``

## indexOf(), lastIndexOF()

Wyszukuje pozycję index pierwszego wystąpienia elementu

`array.indexOf(item, start)`

Wyszukuje pozycję index ostatniego wystąpienia elementu

`array.lastIndexOf(item, start)`

## find()

Zwraca pierwszy element tablicy, który spełnia warunek podanej funkcji testującej.

`array.find(element => { condition });`

## reduceRight()

Podobna do reduce, tylko wykonywana od prawej strony tablicy. Wykonuje operację na każdym elemencie, by ostatecznie zwrócić jeden wynik.

`array.reduceRight((total, value, index, array) => {...})`

## Array.from()

Metoda klasy. Podobna do map? Tworzy tablicę z elementu iterowalnego. Wykonuje na nim podaną funkcję.

`Array.from([1, 2, 3], x => x + x); // [2, 4, 6]`

Generowanie ciągu cyfr:

`Array.from({length: 10}, (_, i) => i + 1); // [1, 2, 3, ..., 10]`

## includes()

Sprawdza, czy tablica zawiera wartość.

```js
const fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.includes("Mango"); // is true
```


# Array Const

Słowo kluczowe `const`, określające "stałą" zmienną blokuje możliwość jej nadpisania (w odróżnieniu od `let`). Wprzypadku obiektów - jest to jednak referencja, co umożliwia zmianę zawartości (Object, Array).

UWAGA: nie dotyczy to stringów, również tworzonych przez new String() - leci error.

# Dates

`const d = new Date();`

Defaultowo js korzysta z daty przeglądarki, lokalizacji i języka. Obiekt daty jest statycznym snapszotem daty w momencie jej pobrania.

Datę można wygenerować na 4 sposoby

```js
new Date()
new Date(milliseconds)
new Date(date string)
new Date(year, month, day, hours, minutes, seconds, milliseconds)
```

Uwaga, miesiące liczone są od 0 do 11. 12 zwróci kolejny rok, podobnie dni.

Minimalna liczba argumentów to dwa. Jeden jest traktowany jako milisekundy!

```js
const d = new Date(2018); // Thu Jan 01 1970 ...
const d = new Date(2018, 3); // Sun Apr 01 2018 ...
```

## `Era linuxa`

**Zero time is January 01, 1970 00:00:00 UTC.**


# Date Formats

## default

Podstawowy, defaultowy format daty w JS

`Wed Jul 06 2022 11:18:26 GMT+0200 (czas środkowoeuropejski letni)`

## ISO

Daty mogą być podawane jako stringi w formacie ISO (YYYY-MM-DD) lub (YYYY-MM-DDTHH:MM:SSZ)

```js
const d = new Date("2015-03-25");
const d = new Date("2015-03");
const d = new Date("2015");
const d = new Date("2015-03-25T12:00:00Z");
```
Z jest oznaczeniem czasu UTC. Usunięcie Z pozwala na dodawanie lub usuwanie czasu (+/-HH:MM)

Dopuszczalne są także inne formaty (long).

```js
const d = new Date("03/25/2015");
const d = new Date("2015-03-25");
const d = new Date("Mar 25 2015");
const d = new Date("25 Mar 2015"); // dzień i miesiąc mogą być zamiennie

```

## parsowanie daty do milisekund

`let msec = Date.parse("March 21, 2012");`


# Date Get Methods

Wybrane metody pobierania daty.

`Date.now()`	Aktualny czas ECMA5

`getFullYear()` -	pełny rok jako numer (yyyy)

`getMonth()` -	miesiąc jako numer (0-11)

`getDate()` -	dzień jako numer (1-31)

`getDay()` -	dzień tygodnia jako numer (0-6, 0 = Sunday, niedziela)

`getHours()` -	godzina (0-23)

`getMinutes()` -	minuta (0-59)

`getSeconds()` -	sekunda (0-59)

`getMilliseconds()` -	millisekunda (0-999)

`getTime()` -	czas w milisekundach od ery linuxa

Metody mają swoje wersje UTC (Greenwich). Polska znajduje się w strefach czasowych:

UTC+01:00 (czas środkowoeuropejski (CET), w okresie zimowym) oraz
UTC+02:00 (czas środkowoeuropejski letni (CEST), w okresie letnim).

# Date Set Methods

Metody set pozwalają na ustawienie obiektu czasu, np.

```js
const d = new Date();
d.setFullYear(2020);
```

`setTime()` -	Set the time (milliseconds since January 1, 1970)

`setFullYear()` -	ustawia rok (opcjonalnie miesiąc i dzień)

`setMonth()` -	ustawia miesiąc (0-11)

`setDate()` -	ustawia dzień (1-31)

`setHours()` -	ustawia godzine (0-23)

`setMinutes()` -	ustawia minuty (0-59)

`setSeconds()` -	ustawia sekundy (0-59)

`setMilliseconds()` -	ustawia millisekundy (0-999)

# Compare dates

Daty można porównywać między sobą.

```js
const today = new Date();
const someday = new Date();
someday.setFullYear(2100, 0, 14);

if (someday > today) {...}
```

<a href='../README.md' style='border: 1px solid gold; padding: 5px; color: gold'>← back to README.md</a>
<a href='#top' style='border: 1px solid gold; padding: 5px; color: gold'>↑ back to top</a>
