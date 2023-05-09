<a id='top' href='../../README.md' style='border: 1px solid gold; padding: 5px; color: gold'>← back to README.md</a>

# Document

<a href="#">Browser environment, specs</a> |
<a href="#">DOM tree</a> |
<a href="#">Walking the DOM</a> |
<a href="#">Searching: getElement*, querySelector*</a> |
<a href="#">Node properties: type, tag and contents</a> |
<a href="#">Attributes and properties</a> |
<a href="#">Modifying the document</a> |
<a href="#">Styles and classes</a> |
<a href="#">Element size and scrolling</a> |
<a href="#">Window sizes and scrolling</a> |
<a href="#">Coordinates</a>

## Browser environment, specs

-   Każde środowisko ma własny zakres wykorzystania JS
-   browser dostarcza obiekt `window`, który pozwala na dostęp do DOM, BOM, CSSOM czy samego JS

`window` jest globalnym obiektem reprezentującym okno przeglądarki.

### DOM

**Document Object Model**

-   DOM reprezentuje cały obiekt dokumentu na stronie, który można modyfikować.
-   `document` jest głównym punktem wejścia, który pozwala na dostęp do jego elementów

```js
// change the background color to red
document.body.style.background = 'red';
```

### BOM

**Browser Object Model**

-   reprezentuje środowisko dostarczane przez przeglądarkę (hosta)

BOM zawiera m.in. takie elementy jak:

-   navigator
-   screen
-   location
-   history
-   XMLHttpRequest
-   inne...

### JavaScript

-   pozwala na dynamiczną obsługę strony

Zawiera takie elementy jak:

-   Array
-   Object
-   Function
-   inne...

### CSSOM

-   reprezentuje model styli,
-   pozwala na modyfikację zasad (rules) stosowania styli
-   rzadko używany

---

## DOM tree

Drzewo struktury dokumentu, składającego się z tagów (znaczników) o specyficznych właściwościach.

-   Każdy z nich jest obiektem, który można modyfikować za pomocą JS.
-   `text node` zawiera wyłacznie string i nie może mieć potomków
-   DOM ma możliwość pewnej `autokorekty` w przypadku wadliwego kodu
    -   dodaje tagi zamykające
    -   dodaje `<tbody>`, jeśl go nie ma
    -   przesuwa `</body>` na koniec dokumentu

Przykładowa struktura drzewa

```html
<!DOCTYPE html>
<html>
    <head>
        <title>About elk</title>
    </head>
    <body>
        The truth about elk.
    </body>
</html>
```

---

## Walking the DOM

`document` umożliwia dostęp i manipulację node'ami dokumentu.

Podstawowe elementy

-   `document.documentElement` => `<html>
-   `document.body` => `<body>
-   `document.head` => `<head>

Uwaga: jeśli skrypt jest w nagłówku, a nie jest opóźniony przez `defer`, nie będzie miął dostępu do `<body>`, które jeszcze się nie wczytało.

`Children` - bezpośrenie dzieci node'a

`Descendants` - wszyscy potomkiwie (całe drzewo łącznie z children)

### DOM collections

Struktura drzewa nie jest tablicą. Jest obiektem tablico-podobnym, iteracyjnym. Metody tablicowe nie zadziałają.

Należy użyć `for...of`!

```js
for (let node of document.body.childNodes) {
    alert(node); // shows all nodes from the collection
}
```

Można użyć metod tablicowych pod warunkiem konwersji

```js
Array.from(document.body.childNodes).filter;
```

Dostęp do elementów otoczenia node'a możliwy jest na różne sposoby.

-   `previous/nextSibling`
-   `first/lastChild`
-   `parentNode`

Dostęp do elementów otoczenia node'a - tylko bazowych elementów (bez textNode, comments)

-   `previous/nextElementSibling`
-   `first/lastElementChild`
-   `parentElement`

Tabele (`<table>`) mają także możliwość dostępu do elementów potomnych takich, jak `rows`, `cels` itp.

---

## Searching: getElement*, querySelector*

Na dostęp do wybranych elementów lub ich wyszukiwanie pozwalają niektóre metody.

Uwaga - każde id elementu jest przechowywane w zmiennych globalnych. Dlatego nie mogą się powtarzać oraz - nie nalezy używać ich jako punktu dostępowego. W tym celu nalezy użyć metody `getElementById()`.

Inne metody dostępu:

-   `querySelector`, `querySelectorAll`
-   `matches` - zwraca true/false jeśli element spełnia warunek
-   `closest` - zwraca najbliższego parenta spełniającego warunek

---

## Node properties: type, tag and contents

---

## Attributes and properties

---

## Modifying the document

---

## Styles and classes

---

## Element size and scrolling

---

## Window sizes and scrolling

---

## Coordinates

---

<br/>
<br/>
<br/>

<a href='../../README.md' style='border: 1px solid gold; padding: 5px; color: gold'>← back to README.md</a>
<a href='#top' style='border: 1px solid gold; padding: 5px; color: gold'>↑ back to top</a>
