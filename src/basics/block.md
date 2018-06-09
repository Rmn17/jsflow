# Блок инструкций

В JavaScript блоком (или блоком инструкций) называется набор последовательных инструкций заключенных в фигурные скобки.

```Javascript
{
  инструкция_1;
  инструкция_2;
  ...
  инструкция_n;
}
```

Пример:

```Javascript
var amount = 99.99;

// отдельный блок
{
    amount = amount * 2;
    console.log( amount ); // 199.98
}
```

Такой вид отдельного блока `{ ... }` вполне допустим, но не часто встречается в JavaScript-программах. Блок обычно используется с управляющими инструкциями, такими как `if`, `for`, `while` и прочими.

```Javascript
while (x < 10) { x++; }
```

В вышеприведенном примере `{ x++; }` является блоком.

Еще пример:

```Javascript
var amount = 99.99;

// сумма достаточно велика?
if (amount > 10) {          // <-- блок прикрепляется к `if`
    amount = amount * 2;
    console.log( amount );  // 199.98
}
```

Как вы видите блок `{ ... }` с двумя инструкциями присоединен к `if (amount > 10)`. Инструкции внутри этого блока будут выполнены только если amount будет больше `10`, в ином случае этот блок выполняться не будет.