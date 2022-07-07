<a href='../README.md' style='border: 1px solid gold; padding: 5px; color: gold'>← back to README.md</a>

# Modules, introduction

Moduły to pliki zawierające fragmenty kodu. Mogą być importowane i mogą eksportować funkcje czy wartości na zewnątrz.

`export function sayHi(user) { alert('Hello, ${user}!'); }`

`import {sayHi} from './sayHi.js';` - import następuje relatywnie do ścieżki

## module

Import skryptów wymaga wskazania, że mamy do czynienia z modułami. Uwaga: moduły działają tylko przez http(s), nie lokalnie. Wymagają użycia serwera.

`<script type="module">...<script>`

1. moduły działają w 'strict mode'.
2. moduły mają własny scope, muszą exportować, importować funkcje i zmienne. Nawet jeśli zadeklarowane są w tym samym pliku html (który ma własny scope globalny).
3. Jeśli kilka modułów importuje jakiś, to moduł ten jest uruchamiany tylko przy pierwszym imporcie. Zatem niektóre akcje mogą nie zadziałać w pozostałych importach!
4. moduły zawsze są deffered (ładowane i uruchamiane PO załadowaniu strony html)

## async

Skrypt wewnętrzny inline z atrybutem `async` jest ładowany asynchronicznie i uruchamiany bez czekania na załadowanie html. Przydatne w przypadku skryptów nie blokujących sobą wykonania kodu (realizujących inne zadania)

```html
<!-- all dependencies are fetched (analytics.js), and the script runs -->
<!-- doesn't wait for the document or other <script> tags -->
<script async type="module">
    import { counter } from './analytics.js';
    counter.count();
</script>
```

## external scripts

Zewnętrzne skrypty są ładowane tylko raz, jeśli pochodzą z jednego źródła src=''

Skrypty z zewnętrznego serwera muszą mieć nagłówek CORS. Serwer dostarczający musi zezwalać na ominięcie polityki CORS. Nagłówek `Access-Control-Allow-Origin` zezwalający na fetch.

```html
<!-- another-site.com must supply Access-Control-Allow-Origin -->
<!-- otherwise, the script won't execute -->
<script type="module" src="http://another-site.com/their.js"></script>
```

# Export and Import (static)

## eksporty przed deklaracją

Export obiektu lub zmiennej
`export let months = [1,2,3];`

Export klasy
`export class User {}`

## export poza deklaracją

Eksport na końcu pliku
`export {foo, baar};`

## Importy

Import wybranych elementów
`import {foo, baar} from './file.js'`

Import wszystkich exportów
`import * as user from './userFile.js'`

## as

Export as

```js
// 📁 say.js
export {sayHi as hi, sayBye as bye};
// 📁 main.js
import * as say from './say.js';
say.hi('John'); // Hello, John!
say.bye('John'); // Bye, John!
```

Import as
```js
// 📁 main.js
import {sayHi as hi, sayBye as bye} from './say.js';
hi('John'); // Hello, John!
bye('John'); // Bye, John!
```

## reexport

Reexport pozwala zgromadzić w jednym miejscu (index.js) exporty rzeczy, które chcemy upublicznić.

```js
// 📁 auth/index.js
// re-export login/logout
export {login, logout} from './helpers.js';

// re-export the default export as User
export {default as User} from './user.js';
```

# Dynamic imports

Statyczne importy wymagają stringa jako źródła pliku i nie pozwalają na zastosowanie warunków.

Wyrażenie `import(module)` pozwala na asynchroniczne załadowanie listy eksportów i może być wykorzystane w kodzie.

```js
// 📁 say.js
export default function() {
  alert("Module loaded (export default)!");
}
export function foo() {
  alert(`Hello`);
}
// 📁 index.html
<script>
  async function load() {
    let say = await import('./say.js');
    say.foo(); // Hello!
    say.default(); // Module loaded (export default)!
  }
</script>
```


<a href='../README.md' style='border: 1px solid gold; padding: 5px; color: gold'>← back to README.md</a>
<a href='#top' style='border: 1px solid gold; padding: 5px; color: gold'>↑ back to top</a>
