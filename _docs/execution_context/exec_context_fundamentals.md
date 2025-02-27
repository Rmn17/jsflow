---
title:  Контекст выполнения. Основы
description: Контекст выполнения используется для того, чтобы отслеживать ход выполнения кода. Именно с его помощью определяется доступное окружение на текущем этапе выполнения программы.
order: 14
---

Лексическое окружение позволяет сформировать доступный нам набор переменных и функций, учитывая вложенность кода. Но по ходу выполнения программы этот набор данных изменяется, более того у каждой вызванной функции своё собственное лексическое окружение. Каким образом можно отследить происходящие в лексическом окружении изменения, а также определить какое лексическое окружение является текущим, соответствующим данному этапу выполнения кода? Для этих целей используется контекст выполнения.

**Контекст выполнения (execution context)** в JavaScript используется для того, чтобы отслеживать ход выполнения кода. Именно с его помощью определяется доступное окружение на текущем этапе выполнения программы. А также контекст выполнения содержит в себе дополнительные параметры, которые формируются самостоятельно JavaScript-движком при обработке вашего кода.

_Контекст выполнения тоже является абстрактным механизмом спецификации_, как и лексическое окружение, к которому невозможно напрямую обратиться или изменить из программы. По сути он представляет собой некую обертку для выполняемого кода, содержащую определенные вспомогательные компоненты для отслеживания состояния программы, к некоторым из которых, мы можем обратиться из нашего кода.

Одним из таких динамически устанавливаемых параметров, к которому можно напрямую обратиться из кода и получить доступ к определенному набору данных в рамках текущего контекста выполнения, является ключевое слово _**this**_. Основным назначением ключевого слова `this` является переиспользование связанного с ним кода в рамках различных окружений. Значение, на которое ссылается ключевое слово `this` в том или ином месте программы определяется самим местом и способом создания текущего контекста выполнения. Более подробно механизм `this` будет рассмотрен в последующих частях курса.

Первым контекстом выполнения, который создаётся при запуске JavaScript скрипта является **глобальный контекст выполнения (Global Execution Context)**. В рамках этого контекста будет обрабатываться весь глобальный код программы. Соответственно текущим Лексическим окружением, связанным с глобальным контекстом выполнения, является глобальное глобальное окружение _Global Environment_.

В рамках глобального контекста JavaScript-движок создает **глобальный объект (global object)** c определенными внутренними свойствами, который будет доступен в любой точке программы. Глобальный объект global object может разнится в зависимости от среды выполнения кода. В рамках браузера глобальным объектом будет объект `window`. Но если мы рассмотрим глобальный объект в среде NodeJS, то им уже будет объект `global`. Ключевое слово `this` в рамках глобального контекста выполнения, будет ссылаться именно на этот глобальный объект.

Например, создадим пустой файл JavaScript и страницу html, загружающую этот файл. Файл JavaScript добавляется на страницу с помощью тега `script`

```html
<script src="путь_к_файлу"></script>
```

Подробнее об этом теге можно прочитать [здесь](../intro/programm_launch.md#добавление-javascript-на-страницу-и-запуск-в-браузере). Архив с уже готовыми исходными файлами доступен по [этой ссылке](/source_code/execution_context/empty_script_example.zip).

Открыв эту html страницу в браузере и запустив Консоль Разработчика (F12), можно получить доступ к глобальному объекту, введя в консоли `window` `this`

```javascript
window

> Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, frames: Window, …}
```

Этот глобальный объект был создан JavaScript-движком, несмотря на то, что запускался абсолютно пустой скрипт. И в этом глобальном объекте уже есть определенный набор встроенных, доступных для нашего использования, методов. Развернув в Консоли Разработчика объект `Window`, можно увидеть все доступные методы, например те, что уже использовались нами ранее — `alert` и `prompt`.

Далее, так как в глобальном контексте выполнения ключевое слово `this` указывает на глобальный объект, то введя в консоли `this`, отобразится тот же самый глобальный объект `window`

```javascript
this

> Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, frames: Window, …}
```

Также в JavaScript есть такая особенность, что объявленные в глобальной области видимости функции и `var` переменные помещаются в свойства глобального объекта `window`:

```javascript
// global scope

var foo = "hello";
function bar() {}

console.log(window.foo); // "hello"
console.log(window.foo == this.foo); // true

console.log(window.bar); // ƒ bar(){}
console.log(window.bar == this.bar); // true
```

Создатель JavaScript Брендан Эйх считает глобальный объект одним из своих «самых больших сожалений». Такая особенность глобального объекта отрицательно сказывается на производительности, значительно усложняет реализацию областей видимости переменных и приводит к меньшему модульности кода.

Глобальные переменные имеют два недостатка. Во-первых, части программного обеспечения, которые полагаются на глобальные переменные, подвержены побочным эффектам; они менее надежны, ведут себя менее предсказуемо и менее пригодны для повторного использования.
Во-вторых, весь JavaScript на веб-странице имеют одни и те же глобальные переменные: ваш код, встроенные модули, код аналитики, кнопки социальных сетей и т. д. А значит, может возникнуть конфликт имен переменных в разных частях вашей программы.

Именно поэтому лучше стараться максимально избегать создания глобальных переменных и объявлять переменные в локальных областях видимости, где они будут непосредственно использоваться.

А также с выходом стандарта ES6, где появилось новое объявление переменных через `const` и `let`, объявление переменных через var начало устаревать и всё меньше используется в коде. И объявленные глобально переменные через `const` или `let`, уже не попадут в глобальный объект.

```javascript
let newValue = "hello";
console.log(window.newValue); // undefined
```

Еще одной особенностью является то, что если в коде присвоить значение какой-либо необъявленной ранее переменной и, если этот код выполняется _**не**_ в строгом режиме "use strict", то JavaScript не только не отобразит ошибку, о несуществующей переменной, но и создаст её для нас в глобальном объекте.

```javascript
function foo() {
 // используется переменная undeclared, которая нигде ранее не объявлялась
 undeclared = 5;
 // необъявленная переменная undeclared будет добавлена в глобальный объект
 console.log(window.undeclared); // 5
}
foo();
```

Эта особенность тоже носит негативные эффекты и может привести к возникновению ошибок в процессе выполнения программы. Поэтому переменные всегда необходимо объявлять для их последующего использования. В режиме в строгом режиме "use strict" этот код уже не выполнится и корректно отобразит ошибку

```javascript
"use strict";

function foo() {
 undeclared = 5; // Uncaught ReferenceError: undeclared is not defined
 console.log(window.undeclared);
}
foo();
```

Когда в JavaScript программе кроме глобального кода есть еще вызов функций. Например

```javascript
let value = "value from global scope";

function foo() {
 let fooValue = "value from foo function";
}
foo();
```

То при каждом новом вызове функции создаётся свой контекст выполнения, при этом текущий контекст выполнения (тот, откуда была вызвана функция) приостанавливается.

В предыдущем примере, когда мы из глобального контекста вызываем функцию, обработка глобального контекста приостанавливается и управление переходит в контекст выполнения этой функции. Важно отметить, что в определенное время может обрабатываться только один контекст выполнения. Далее, после выполнения этой функции необходимо вернуться обратно в глобальный контекст, для выполнения следующего кода программы.

Чтобы хранить и отслеживать контексты выполнения они формируются в **стек контекстов выполнения (call stack)** — список контекстов, организованных по принципу «последним пришёл — первым вышел». Выполняемый контекст всегда является верхним элементом этого стека. После того, как необходимый код выполнится, связанный с ним контекст выполнения удалится, управление вернется в контекст, который находился элементом ниже, и теперь он будет верхним элементом — текущим контекстом выполнения.

Для предыдущего примера стек контекстов выполнения в ходе программы схематично можно показать так:

![схема контекстов выполнения](/assets/images/execution_context/execContext1.png)

На этой схеме голубым цветом обозначен текущий, выполняющийся в данный момент времени контекст. А серым — приостановленные контексты, ожидающие окончания выполнения текущего контекста.

Если функция была вызвана из другой функции

```javascript
var x = 10;

function foo() {
 var y = 20;

 function bar() {
  var z = y + x;
 }
 bar();
}
foo();
```

то стек контекстов выполнения будет меняться так:

![схема контекстов выполнения](/assets/images/execution_context/execContext2.png)
