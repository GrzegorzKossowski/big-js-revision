<a href='../README.md' style='border: 1px solid gold; padding: 5px; color: gold'>← back to README.md</a>

# Co to jest JS, po co, dlaczego, jak?

JS obsługuje dynamikę stron www

Jest obsługiwany jako skrypt tekstowy bez prekompilacji

Może być używany w każdym przystosowanym do tego środowisku (np. serwer, urządzenia itp)

Przeglądarki używają "maszyn wirtualnych" do obsługi JS (np. V8 - Chrome)

1. silnik czyta (parsuje) kod
2. silnik kompiluje (konwertuje) skrypt na kod maszynowy
3. maszyna realizuje kod

## Działanie i ograniczenia JS

1. pozwala na pełną modyfikację strony www (DOM)
2. reaguje na zachowanie użytkownika
3. zapisuje lokalnie w ograniczonym zakresie (cookies, storage, BD)
4. pozwala na kontakt z serwerem

---

5. nie pozwala na niskopoziomowy dostęp do systemu, pliktów itp. (co najwyżej zapis pliku, wysłanie pliku)
6. defaultowo realizuje politykę CORS - z zasady stosuje sandbox - zakładki nie wiedzą o sobie i swoich danych, szczególnie jeśli pochodzą z różnych domen, adresów

## SOP & CORS

<a href="https://sekurak.pl/czym-jest-cors-cross-origin-resource-sharing-i-jak-wplywa-na-bezpieczenstwo/">Czym jest CORS</a>

### Same-Origin Policy (SOP)

-   taki sam protokół
-   taki sam host
-   taki sam port

```
np. http://127.0.0.1:5000 albo https://www.google.com
```

# Fundamentals

## Where To

Kod wstawiany jest na stronie za pomocą tagu `<script>`

```js
<script>
    document.getElementById("demo").innerHTML = "My First JavaScript";
</script>
```

Skrypty można umieszczać zarówno w nagłówku - headerze, jak i w `<body>` dokumentu. Najlepszym rozwiązaniem jest umieszczanie skryptów na końcu body, bo ich przetwarzanie blokuje proces.
Jeśli w headerze, to z atrybutem defer, co blokuje przetwarzanie skryptu

Jeśli `src` jest ustawione w `script`, to wyższość ma import i kod wewnątrz jest pomijany.

```js
<script src='myScript.js' defer>
    //...some code
</script>
```

Najlepszym rozwiązaniem jest trzymanie kodu w plikach zewnętrznych, co umożliwia keszowanie plików w przeglądarce i pobieranie równoległe.

## Składnia

**Średniki**

Średniki nie są wymagane o ile osobne wyrażenia nie występują w jednej linii. JS dodaje automatycznie średniki podczas obróbki kodu. Dobrą praktyką jest jednak dodawanie średników w przypadku możliwości wystąpienia błędów podczas parsowania czy kompilacji.

Przykład błednego kodu

```javascript
alert('Hello')[
    // tu nastąpi błędna interpretacja => alert("...")[...]
    (1, 2)
].forEach(alert);
```

## Use strict

`use strict` wprowadzono, aby oddzielić nowoczesny kod od starego, celem uniknięcia błedów wstecznych.
Korzystanie z klas czy modułów włącza `use strict` atuomatycznie

```javascript
"use strict";

// this code works the modern way
...
```

## Output

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

## Variables

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

### Let

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

### Const

-   Wprowadzone w 2015 r.
-   Nie może być deklarowana ponownie.
-   Nie mogą być przypisane ponownie (const - stałe)
-   Dotyczą bloku → { const }

Musi być zadeklarowana i zainicjalizowana. Deklaruje się stałą referencję a nie wartość. Pozwala na zmiany w obiektach (array itp).

`const PI = 3.14159265359;`

# Data types (typy danych)

## Number

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

Specjalne typy number to `+Infinity`, `-Infinity`, `NaN` - również typu **number**.

## NaN

Reserved word określające, że numer nie jest legalny. Jest wynikiem operacji na niedozwolonych numerach, takich jak dzielenie przez zero lub stringa nienumerycznego itp.

Uwaga: NaN jest typu number → typeof NaN → number!

```js
let x = 100 / 'Apple'; // → NaN
let x = 100 / '10'; // → 10 (poprawne!)
let z = x + NaN; // → NaN
typeof NaN; // → number
```

## BigInt

Specjalny typ dla danych numerycznych wykraczających poza bezpieczny zakres +/- 2<sup>53</sup> lub 64 bit.
Rzadko używane, do precyzyjnych obliczeń lub do kosmicznych wartości.

Tworzy się przez dodanie na końcu n

```javascript
const big = 353543534....53454534n
```

---

## Strings

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

## String Methods

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

## String Search

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

## String Templates

Templaty uzyskujemy za pomocą \` (back-ticków). Pozwalają na wstawianie zmiennych przy użyciu `${}`

---

## Boolean

Reprezentuje wartości z zakresu `true` oraz `false`.

## NULL

Null to obiekt specjalnego przeznaczenia, reprezentujący brak wartości. `typeof` wskazuje na object, ale jest to błąd wynikający z zaszłości.

Null jest (powinien być) typem samym w sobie (czyli typu null)


## Undefined

Reprezentuje zmienną o niezdefiniowanej wartości.
Np.:
```javascript
let noDefined; // => undefined
noDefined = 3 // => defined 
```
---

## Objects

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

---

## Operatory typu

`typeof` - zwraca typ zmiennej

`instanceof` - zwraca true, jeśli obiket jest instancją typu

<a href='../README.md' style='border: 1px solid gold; padding: 5px; color: gold'>← back to README.md</a>
<a href='#top' style='border: 1px solid gold; padding: 5px; color: gold'>↑ back to top</a>

<a href='./jsFundamentals2.md' style='border: 1px solid gold; padding: 5px; color: gold'>Next →</a>
