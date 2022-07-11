<a href='../README.md' style='border: 1px solid gold; padding: 5px; color: gold'>← back to README.md</a>

# Introduction: callbacks

# Promise

Konstruktor Promisa. Funkcja podana, executor, wykonuje się automatycznie. JS dostarcza dwa callbacki - resolve i reject.
Resolve dostaje wynik działania egzekurota, reject dostaje ew. error.

```js
let promise = new Promise(function (resolve, reject) {
    // executor
});
```

Obsługa przez funkcję then.

```js
// resolve runs the first function in .then
promise.then(
    result => alert(result), // shows "done!" after 1 second
    error => alert(error) // doesn't run
);
```

Równoważny zapis to

```js
// reject runs the second function in .then
promise.then(
    null,
    error => alert(error) // reject runs the second function in .then
);
```

## catch()

Równoważny zapis to wykorzystanie catch()

```js
promise.catch(error => alert(error));
```

`then(null, foo)` jest równoważny zapisowi `catch(foo)`

UWAGA: takie zapisy NIE są równoważne.

`promise.then(f1).catch(f2);` vs `promise.then(f1, f2);`

Pierwsza wersja złapie błąd z f2 w f2. Druga werjs nie złape błędu, bo ten jest przekazywany dalej, a dalej nic nie ma.

## finally()

`finally` ma za zadanie posprzątać bez wzgledu na wynik, nie przyjmuje argumentów, jest przezroczyste dla procesu (może przekazać wyniki dalej). Zadaniem finally jest pozamykać generalne zadania (otwarte procesy, połaczenia itp.). Jedyne, co może przekazać dalej, to error obrabiany w kolejnym catchu.

```js
new Promise((resolve, reject) => {
    setTimeout(() => resolve('value'), 2000);
})
    .finally(() => alert('Promise ready')) // triggers first
    .then(result => alert(result)); // <-- .then shows "value"
```

# Promises chaining

Tylko taki zapis

`new Promise().then().then()...`

przekazuje dane w ciągu i pozwala je obrabiać.

taki zapis nie powoduje przekazania w dół danych. Wszystkie korzystają z pierwszego resolve()!

```js
promise = new Promise();
//.... resolve() jest taki sam dla wszystkich.
promise.then();
promise.then();
promise.then();
```

Promis może sam zwracać promisa. Wtedy następne bloki czekają na jego wykonanie.

```js
new Promise(function(resolve, reject) {
  resolve()
}).then(function(result) {
  return new Promise((resolve, reject) => {
    resolve()
  });
}).then(function(result) {
  // czekają na środkowy promis
  }
```

## summary

.then(handler) zwraca promise

1. if return value => state: fulfilled, result: value
2. if throw error => state: rejected, result: error
3. if return promise => czeka na ten promis

# Error handling with promises

Podstawowy sposób przechwytywania błędów. Catch().

```js
fetch('https://no-such-server.blabla') // rejects
    .then(response => response.json())
    .catch(err => alert(err)); // TypeError: failed to fetch (the text may vary)
```

Rzucanie błędu jest równoznaczne z rejectem.

`throw new Error("Whoops!");` => `reject(new Error("Whoops!"));`

## Rethrowing

Jeśli `catch()` nie może obsłużyć błędu, może rzucić go do następnego bloku `catch`.

```js
.catch(function(error) { // (*)
  if (error instanceof URIError) {
    // handle it
  } else {
    alert("Can't handle such error");
    throw error; // throwing this or another error jumps to the next catch
  }
}).then(function() {
  /* doesn't run here */
}).catch(error => { // (**)
  alert(`The unknown error has occurred: ${error}`);
  // don't return anything => execution goes the normal way
});
```

Nieobsłużony błąd jest wynoszony do globalnego poziomu.

# Promise API

## Promise.all

Promise.all czeka na wykonanie wszystkich promis podanych jako parametry, zwraca tablicę wyników ich działania.

`let promise = Promise.all(iterable);`

Kolejność tablicy odpowiada kolejności parametrów, bez wzgędu na czas ich wykonania.

Error w którymkolwiek powoduje natychmiastowy error w całości. Zasada jest wszystko albo nic. Pozostałe promisy są ignorowane. NIESTETY, jeśli we wszystkich są jakieś procesy, są kontynuowane.

**Zwraca:** Tablica wyników dla wszystkich promis lub error.

## Promise.allSettled

Nowa (nie działa na starych przeglądarkach) metoda `allSettled()` zwraca wszystkie wyniki w tablicy jako obiekty, nie zwraca błędu. Zastąpiła polyfill.

```js
[
 {status:"fulfilled", value:result},  //  dla poprawnych odpowiedzi,
 {status:"rejected", reason:error},   // dla błędnych odpowiedzi.
 ...
]
```

**Zwraca:** Tablica wyników dla wszystkich promis.

## Promise.race(iterable)

`let promise = Promise.race(iterable);` podobna do Promise.all. Zwraca jednak pierwszą z promis - bez względu na wynik, czy success czy error.

**Zwraca:** pierwszy wykonany lub error.

## Promise.any(iterable)

`let promise = Promise.any(iterable);` zwraca pierwszą promisę, która się powiodła. Jeśli wszystkie dadzą error, wtedy dopiero rzuca tablicą errorów dla wszystkich promis.

**Zwraca:** pierwszy poprawny lub tablica błędów.

# Promisification

Zamiana funkcji z callbackiem na promisy
https://javascript.info/promisify

# Microtasks

Promisy oraz ich handlery są odkładane na kolejkę fifo.
Kolejka ta jest uruchamiana dopiero, kiedy inne taski z bieżącego kodu zostaną wykonane.

```
promise.then( handler ); // => handler wrzucony na kolejkę
alert ( "code finished" );  // => zwykły kod wykonywany
// ----------------------------
// => script execution finished here
// => zadanie z kolejki wykonywane (wynik promisa)
```

# Async/await

## async

Async wskazuje, że funkcja zawsze zwraca promisę.

```js
async function f() {
    return 1;
}
f().then(alert); // 1
```

## await

`await` sprawia, że JS czeka na wykonanie się promisy. 

```js
const result = await fetch(url)
const json = await result.json()
```
`await` wymaga, by funkcja była `async`. Wyjątkiem jest moduł, który pozwala na wykorzystanie async na globalnym poziomie.

`let response = await fetch('/article/promise-chaining/user.json');`

Starsze przeglądarki wymagają użycia IIF.

```js
(async () => {
  let response = await fetch('/article/promise-chaining/user.json');
  let user = await response.json();
  ...
})();
```

W klasach można używać `async` bezpośrednio

```js
class Waiter {
  async wait() {
    return await Promise.resolve(1);
  }
}
```

Normalnie `await` zwraca wynik. W przypadku błędu rzuca Error, tak jakby był on normalnie rzucony w linii

```js
async function f() {
  await Promise.reject(new Error("Whoops!"));
}
///…is the same as this:
async function f() {
  throw new Error("Whoops!");
}
```

Rzucenie błędu może zająć czas. W związku z tym należy użyć bloku `try-catch`. Może on obejmować cały kod z `await`.
```js
async function f() {

  try {
    let response = await fetch('http://no-such-url');
    let user = await response.json();
  } catch(err) {
    alert(err); // TypeError: failed to fetch
  }
}

f();
```

W przypadku braku catch wewnątrz funkcji, błąd przenoszony jest wyżej - do funkcji `async`. Można ją również catch'ować.

`async function f() {...}.catch()`


https://javascript.info/async-await

<a href='../README.md' style='border: 1px solid gold; padding: 5px; color: gold'>← back to README.md</a>
<a href='#top' style='border: 1px solid gold; padding: 5px; color: gold'>↑ back to top</a>
