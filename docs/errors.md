<a href='../README.md' style='border: 1px solid gold; padding: 5px; color: gold'>← back to README.md</a>

# Error handling, "try...catch"

## try{}

Do łapania błędów używany jest blok try-catch. Try wykonuje skrypt. Jeśli rzuci błąd, jest on łapany i przetwarzany w catch.

```js
try {
   ... try to execute the code ...
} catch (err) {
   ... handle errors ...
} finally {
   ... execute always ...
}
```
Proces - JS przetwarza skrypt (pierwsze czytanie), potem wykonuje skrypt (drugie czytanie). Blok try-catch musi przejść poprawnie pierwsze czytanie. Jeśli błąd jest typu `parse-time` nie zostanie nawet uruchomiony. Dopiero w fazie `run-time` blok `try` zostanie "poddany obróbce" i jeśli wtedy rzuci błąd, złapie go `catch`.

Try-catch jest **synchroniczne**!!! Tzn. nie obsługuje błedu, który wydarzy się w kodzie odłożonym na asynchroniczną kolejkę.

Uwaga = promisy mają wewnętrzny sekretny try-catch, dlatego wyłapują błędy.

## catch(){}

Kiedy błąd wystąpi, JS generuje obiekt zawierający dane na jego temat. Jest przekazywany do catch(error).

Obiekt błędu zawiera:

`name` - nazwa błędu

`message` - komunikat (nadawany przez porgramistę)

`stack` - niestandardowa informacja dotycząca sekwencji wywołań błędu.

Uwaga, w najnowszym wydaniu catch() nie wymaga podania error.

## rzucanie błędów

Standardowe błędy:

`Error` - Ogólny błąd

`RangeError` - numer spoza zakresu

`ReferenceError` - nielegalna, niewłaściwa referencja, zmienna, która nie została zadeklarowana

`SyntaxError` - błędna składnia

`TypeError` - niewłaściwe wykorzystanie typu

`URIError` - niewłaściwe znaki w metodzie encodeURI()

Ponadto występują nieliczne błedy producentów, np. Mozilla.

## rethrowing errors

Catch(err) łapie wszystkie błędy, nawet te niezamierzone. Dlatego powinno się obsługiwać konkretny, znany błąd, a resztę pchać do kolejnego bloku catch(), aż do wyczerpania inwencji. Dzięki temu możemy obsłużyć jak najwięcej błędów.

Sprawdzanie typu błędu:

`instanceof` - sprawdzamy, jakiego typu to błąd.

`err.name` - sprawdzamy klasę błędu z nazwy. Również `err.constructor.name`

`
## finally{}

Finally wykonuje się zarówno po wykonaniu try, jak i po wykonaniu catch. Używane do finalizowania procesów, bez względu na wynik try-catch.

Finally zadziała nawet, jeśli w try zostanie użyty return.

## błędy globalne

Nie ma tego w specyfikacji, ale przelądarki dostarczają metody window.onerror, która pozwala na obsługę błędów globalnych.


# Custom errors, extending Error

Wiele sytuacji wymaga customowych błędów. Można je tworzyć, rozszerzając klasę Error.

```js
class CustomError extends Error {
  constructor(message) {
    super(message); // odwołujemy się do parenta (Error)
    this.name = "CustomError"; // nadpisujemy nazwę błędu.
  }
}
```
Wykorzystanie nowej klasy błędu:

```js
try {
  // ... kod rzucający szczegółowy błąd
} catch (err) {
  if (err instanceof ValidationError) {
    alert("Invalid data: " + err.message); // Invalid data: No field: name
  } else if (err instanceof SyntaxError) { // (*)
    alert("JSON Syntax Error: " + err.message);
  } else {
    throw err; // unknown error, rethrow it (**)
  }
}
```

Wykorzystanie `instanceof class` pozwala na późniejsze rozszerzanie klas o nowe.

W jaki sposób przypisać nazwę klasy automatycznie z konstruktora.
Należy stworzyć klasę bazową rozszerzającą Error, a potem rozszerzać z niej.

```js
class CustomError extends Error {
  constructor(message) {
    super(message);
    this.name = this.constructor.name; // tu przypisujemy nazwę klasy
  }
}

class ValidationError extends MyError { 
  //... tu jakaś nowa klasa
}

// tu inna klasa rozszerzająca poprzednią
class PropertyRequiredError extends ValidationError {
  constructor(property) {
    super("No property: " + property);
    this.property = property;
  }
}

// name is correct
alert( new PropertyRequiredError("field").name ); // PropertyRequiredError
```

## wrapping exceptions

Tworzenie wrapera mniejszych errorów ma na celu ich zgrupowanie i wydzielenie abstrakcji wyższego rzędu.
Jeśli funkcja wygeneruje lub przejmie błąd danego typu (klasy), rzuca błąd ogólny (wyższego rzędu) z nazwą i ew. referencją do tego typu niższego.

błąd niższego rzędu => wyłapujemy go i rzucamy błąd wyższego rzędu => ostatecznie wyłapujemy go i sprawdzamy jego powód (cause)

```js
class ReadError extends Error {
  constructor(message, cause) {
    super(message);
    this.cause = cause;
    this.name = 'ReadError';
  }
}

// potem
try {
    user = JSON.parse(json);
  } catch (err) {
    if (err instanceof SyntaxError) {
      throw new ReadError("Syntax Error", err);
    } else {
      throw err;
    }
  }

// potem

try {
  readUser('{bad json}');
} catch (e) {
  if (e instanceof ReadError) {
    alert(e);
    // Original error: SyntaxError: Unexpected token b in JSON at position 1
    alert("Original error: " + e.cause);
  } else {
    throw e;
  }
}


```


<a href='../README.md' style='border: 1px solid gold; padding: 5px; color: gold'>← back to README.md</a>
<a href='#top' style='border: 1px solid gold; padding: 5px; color: gold'>↑ back to top</a>
