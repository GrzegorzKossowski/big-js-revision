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
let x = 2;    // Allowed
{
let x = 3;    // Allowed
}
{
let x = 4;    // Allowed
}
```

Let jest hoistowane do góry, ale nie inicjalizowane, więc wywala error

Var jest hoistowane z globalnym zasięgiem, więc można zadeklarować potem

# Const

- Wprowadzone w 2015 r.
- Nie może być deklarowana ponownie.
- Nie mogą być przypisane ponownie (const - stałe)
- Dotyczą bloku → { const }

Musi być zadeklarowana i zainicjalizowana. Deklaruje się stałą referencję a nie wartość. Pozwala na zmiany w obiektach (array itp).

```const PI = 3.14159265359;```

Zmienne const są hoistowane i mogą być deklarowane po inicjalizacji, w obrębie scope’u.

# Operators

https://www.w3schools.com/js/js_operators.asp

# Arithmetic

# Assignment

# Data Types

# Functions

# Objects

# Events

# Strings

# String Methods

# String Search

# String Templates

# Numbers

# Number Methods

# Arrays

# Array Methods

# Array Sort

# Array Iteration

# Array Const

# Dates

# Date Formats

# Date Get Methods

# Date Set Methods

# Math

# Random

# Booleans

# Comparisons

# If Else

# Switch

# Loop For

# Loop For In

# Loop For Of

# Loop While

# Break

# Iterables

# Sets

# Maps

# Typeof

# Type Conversion

# Bitwise

# RegExp

# Errors

# Scope

# Hoisting

# Strict Mode

# this Keyword

# Arrow Function

# Classes

# Modules

# #ON

# Debugging

# Style Guide

# Best Practices

# Mistakes

# Performance

# Reserved Words
