# jquery.inputmask 3.x

Copyright (c) 2010 - 2016 Robin Herbots Licensed under the MIT license ([http://opensource.org/licenses/mit-license.php](http://opensource.org/licenses/mit-license.php))

[![NPM Version][npm-image]][npm-url] [![Dependency Status][david-image]][david-url] [![devDependency Status][david-dev-image]][david-dev-url]

jquery.inputmask это jQuery плагин для создании маски ввода.

Маска ввода помогает пользователю с вводом, обеспечивая заранее определенный формат. Это может быть полезно для дат, числел, номеров телефонов...

Ососбенности:
- легкое использование
- опциональные части маски
- возможность определить псевдоним, который будет скрывать символы маски
- маски для даты и времени
- маски для чисел
- много функций обратного вызова
- non-greedy маски
- многие функции можно включить/отключить, сконфигурировать опционально
- поддержка readonly/disabled/dir="rtl" атрибутов
- поддержка data-inputmask атрибутов
- генератор - маска
- регулярные выражения в маске
- динамические маски
- предварительная обработка масок
- JIT-masking
- форматирование значений / проверка без поля ввода
- поддержка AMD/CommonJS
- библиотеки зависимостей: vanilla javascript, jQuery, jqlite

Demo page see [http://robinherbots.github.io/jquery.inputmask](http://robinherbots.github.io/jquery.inputmask)

[![donate](https://www.paypalobjects.com/en_US/i/btn/btn_donate_SM.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=ZNR3EB6JTMMSS)

## Применение:
Подключите JS файлы, которые вы можете найти в папке `dist`.

с помощью класса Inputmask

```html
<script src="jquery.js"></script>
<script src="inputmask.js"></script>
<script src="inputmask.???.Extensions.js"></script>
```

```javascript
var selector = document.getElementById("selector");

var im = new Inputmask("99-9999999");
im.mask(selector);

Inputmask({"mask": "(999) 999-9999", .... other options .....}).mask(selector);
Inputmask("9-a{1,3}9{1,3}").mask(selector);
Inputmask("9", { repeat: 10 }).mask(selector);
```

с помощью jquery плагина

```html
<script src="jquery.js"></script>
<script src="inputmask.js"></script>
<script src="inputmask.???.Extensions.js"></script>
<script src="jquery.inputmask.js"></script>
```

или с помощью входящей в комлект поставки версии

```html
<script src="jquery.js"></script>
<script src="jquery.inputmask.bundle.js"></script>
```

```javascript
$(document).ready(function(){
  $(selector).inputmask("99-9999999");  //static mask
  $(selector).inputmask({"mask": "(999) 999-9999"}); //specifying options
  $(selector).inputmask("9-a{1,3}9{1,3}"); //mask with dynamic syntax
});
```

с помощью data-inputmask атрибута

```html
<input data-inputmask="'alias': 'date'" />
<input data-inputmask="'mask': '9', 'repeat': 10, 'greedy' : false" />
<input data-inputmask="'mask': '99-9999999'" />
```

```javascript
$(document).ready(function(){
  $(":input").inputmask();
  or
  Inputmask().mask(document.querySelectorAll("input"));
});
```

Любая опция также может быть передана через использование data-атрибута. Используйте data-inputmask-<**_имя опции_**>="value"

```html
<input id="example1" data-inputmask-clearmaskonlostfocus="false" />
<input id="example2" data-inputmask-regex="[a-za-zA-Z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-zA-Z0-9!#$%&'*+/=?^_`{|}~-]+)*@(?:[a-zA-Z0-9](?:[a-zA-Z0-9-]*[a-zA-Z0-9])?\.)+[a-zA-Z0-9](?:[a-zA-Z0-9-]*[a-zA-Z0-9])?" />
```

```javascript
$(document).ready(function(){
  $("#example1").inputmask("99-9999999");
  $("#example2").inputmask("Regex");
});
```

Если вы хотите автоматически привязать маску ввода для отметки ввода с data-inputmask- ... атрибутами, вы можете включить inputmask.binding.js

```html
...
<script src="inputmask.binding.js"></script>
...
```

Если вы используете модуль загрузки requireJS

Добавьте в ваш config.js

```javascript
paths: {
  ...
  "inputmask.dependencyLib": "../dist/inputmask/inputmask.dependencyLib.jquery",
  "inputmask": "../dist/inputmask/inputmask",
  ...
}
```

Библиотеки зависимостей вы можете выбрать между поддерживаемыми библиотеками.
- inputmask.dependencyLib (vanilla)
- inputmask.dependencyLib.jquery
- inputmask.dependencyLib.jqlite
- .... (другие приветствуются)

### Разрешенные HTML-элементы
- `<input type="text">`
- `<input type="tel">`
- `<input type="password">`
- `<div contenteditable="true">` (и все остальные при поддержке contenteditable)
- `<textarea>`
- любой html-элемент (текстовое содержимое маски или установка значения маски с jQuery.val)

Допустимые типы ввода определяются в опции supportsInputType. Также смотрите ([input-type-ref])

### Символы для маски по умолчанию
- `9` : цифры
- `a` : буквы алфавита
- `*` : буквы и цифры

Есть несколько символов для маски ввода, определенных в ваших расширениях.<br>Вы можете найти информацию в JS-файлах или путем дальнейшего изучения опций.

## Типы масок
### Статические маски
Это очень простой базовый тип маски. Маска определяется и не будет меняться во время ввода.

```javascript
$(document).ready(function(){
  $(selector).inputmask("aa-9999");  //static mask
  $(selector).inputmask({mask: "aa-9999"});  //static mask
});
```

### Дополнительные маски
Можно определить некоторые части маски, как не обязательные.  Это делается с помощью [ ].

Пример:

```javascript
$('#test').inputmask('(99) 9999[9]-9999');
```

Эта маска разрешает ввод как `(99) 99999-9999` или `(99) 9999-9999`.

Input => 12123451234      mask => (12) 12345-1234    (trigger complete)<br>
Input => 121234-1234      mask => (12) 1234-1234     (trigger complete)<br>
Input => 1212341234       mask => (12) 12341-234_    (trigger incomplete)

#### Пропуск необязательной части символов
В качестве дополнительного существует еще один конфигурируемый символ, который используется для пропуска дополнительной части в маске.

```javascript
skipOptionalPartCharacter: " "
```

Input => 121234 1234      mask => (12) 1234-1234     (trigger complete)

Когда `clearMaskOnLostFocus: true` устанавливается в настройках (по умолчанию), маска уберет необязательную часть, когда она не заполнена, и это только в случае, если необязательная часть находится в конце маски.

Например:

```javascript
$('#test').inputmask('999[-AAA]');
```

В то время как поле находится в фокусе или не заполнено, пользователи будут видеть маску `___-___`. Когда требуемая часть маски заполнена и поле теряет фокус, пользователь увидит `123`. Когда обе части - обязательна и не обязательная части маски заполняются и поле теряет фокус, пользователь увидит `123-ABC`.

#### Дополнительные части маски с greedy false
При определении опциональной части маски вместе с greedy: false опцией, inputmask покажет наименьшую возможную часть маски в качестве входных данных первого.

```javascript
$(selector).inputmask({ mask: "9[-9999]", greedy: false });
```

Исходная маска будет показана "**_**" вместо "**_**-____".

### Динамические маски
Динамические маски могут изменяться в процессе ввода.  Для определения динамической части маски используется { }.

{n} => n повторов<br>{n,m} => от n до m повторов

Кроме того {+} и {*} тоже допускаются. + начинаются с 1 и * начинаются с 0.

```javascript
$(document).ready(function(){
  $(selector).inputmask("aa-9{4}");    //статическая маска с динамическим синтаксисом
  $(selector).inputmask("aa-9{1,4}");  //динамическая маска ~ 9 символов могут быть от 1 до 4 раз

  //email маска
  $(selector).inputmask({
    mask: "*{1,20}[.*{1,20}][.*{1,20}][.*{1,20}]@*{1,20}[.*{2,6}][.*{1,2}]",
    greedy: false,
    onBeforePaste: function (pastedValue, opts) {
      pastedValue = pastedValue.toLowerCase();
      return pastedValue.replace("mailto:", "");
    },
    definitions: {
      '*': {
        validator: "[0-9A-Za-z!#$%&'*+/=?^_`{|}~\-]",
        cardinality: 1,
        casing: "lower"
      }
    }
  });
});
```

### Генератор маски
Синтаксис подобен **OR** элементу.  Маска может быть 1 из 2 вариантов, указанных в генераторе.

Для того, чтобы определить генератор, используйте |.<br>например: "a|9" => a или 9<br>    "(aaa)|(999)" => aaa или 999

Кроме того, убедитесь, что прочитали о возможностях keepStatic опции.

```javascript
$("selector").inputmask("(99.9)|(X)", {
  definitions: {
    "X": {
      validator: "[xX]",
      cardinality: 1,
      casing: "upper"
    }
  }
});
```

или

```javascript
$("selector").inputmask({
  mask: ["99.9", "X"],
  definitions: {
    "X": {
      validator: "[xX]",
      cardinality: 1,
      casing: "upper"
    }
  }
});
```

### Предварительная обработка маски
Вы можете задать маску как функцию, которая может позволить предварительно получить маску. Например, сортировка для нескольких масок или извлечения определений маски динамически с помощью AJAX. Предварительная обработка функции должна возвращать действительное определение маски.

```javascript
$(selector).inputmask({ mask: function () { /* do stuff */ return ["[1-]AAA-999", "[1-]999-AAA"]; }});
```

### JIT Masking
Маска в реальном времени.  С помощью опции JIT маски можно включить JIT маску. Маска будет видна для пользователя, только при вводе символов.
По умолчанию: false

Значение может быть true или начальное число или false.

```javascript
Inputmask("date", { jitMasking: true }).mask(selector);
```

## Определение пользовательского символа маски
Вы можете определить свои собственные символы для использования в вашей маске. <br> Начните с выбора символа маски.

### валидатор(chrs, maskset, pos, strict, opts)
Затем определите свой валидатор.  Проверка может быть регулярным выражением или функцией.

Возвращаемое значение валидатор может быть истинным, ложным или объектом.

#### Опциональные команды
- pos : позиция для вставки
- c : символ для вставки
- caret : позиция каретки
- remove : позиция (позиции) для удаления
  - pos или [pos1, pos2]

- insert : позиции (позиций) для добавления :
  - { pos : позиция вставки, c : символ для вставки }
  - [{ pos : позиция вставки, c : символ для вставки }, { ...}, ... ]

- refreshFromBuffer :
  - true => refresh validPositions from the complete buffer
  - { start: , end: } => refresh from start to end

### cardinality
Cardinality определяет, сколько символов представлены и утверждены для определения.

### prevalidator(chrs, maskset, pos, strict, opts)
Опция предварительной проверки используется для проверки символов перед кардинальным определением, которое будет достигнуто. (См 'J' пример)

### definitionSymbol
При вставке или удалении символов, они смещаются только тогда, когда определение типа является то же самое. Такое поведение может быть отменено, давая definitionSymbol. (См пример х, у, z, который может быть использован для IP-адреса маски, проверка отличается, но допускается сдвиг символов между определениями)

```javascript
Inputmask.extendDefinitions({
  'f': {  //masksymbol
    "validator": "[0-9\(\)\.\+/ ]",
    "cardinality": 1,
    'prevalidator': null
  },
  'g': {
    "validator": function (chrs, buffer, pos, strict, opts) {
      //do some logic and return true, false, or { "pos": new position, "c": character to place }
    }
    "cardinality": 1,
    'prevalidator': null
  },
  'j': { //basic year
    validator: "(19|20)\\d{2}",
    cardinality: 4,
    prevalidator: [
      { validator: "[12]", cardinality: 1 },
      { validator: "(19|20)", cardinality: 2 },
      { validator: "(19|20)\\d", cardinality: 3 }
    ]
  },
  'x': {
    validator: "[0-2]",
    cardinality: 1,
    definitionSymbol: "i" //это позволяет сдвига значениt из других определений, с тем же символом маски или определения символа
  },
  'y': {
    validator: function (chrs, buffer, pos, strict, opts) {
      var valExp2 = new RegExp("2[0-5]|[01][0-9]");
      return valExp2.test(buffer[pos - 1] + chrs);
    },
    cardinality: 1,
    definitionSymbol: "i"
  },
  'z': {
    validator: function (chrs, buffer, pos, strict, opts) {
      var valExp3 = new RegExp("25[0-5]|2[0-4][0-9]|[01][0-9][0-9]");
      return valExp3.test(buffer[pos - 2] + buffer[pos - 1] + chrs);
    },
    cardinality: 1,
    definitionSymbol: "i"
  }
});
```

### placeholder
Укажите заполнитель для определения

### set defaults
Значения по умолчанию могут быть установлены, как показано ниже.

```javascript
Inputmask.extendDefaults({
  'autoUnmask': true
});
Inputmask.extendDefinitions({
  'A': {
    validator: "[A-Za-z\u0410-\u044F\u0401\u0451\u00C0-\u00FF\u00B5]",
    cardinality: 1,
    casing: "upper" //автоматический перевод в верхний регистр
  },
  '+': {
    validator: "[0-9A-Za-z\u0410-\u044F\u0401\u0451\u00C0-\u00FF\u00B5]",
    cardinality: 1,
    casing: "upper"
  }
});
Inputmask.extendAliases({
  'Regex': {
    mask: "r",
    greedy: false,
    ...
  }
});
```

Но если свойство определяется в качестве псевдонима необходимо установить его для определения псевдонима.

```javascript
Inputmask.extendAliases({
  'numeric': {
    allowPlus: false,
    allowMinus: false
  }
});
```

Тем не менее, предпочтительный способ, чтобы изменить свойства псевдонима путем создания нового псевдонима, который наследуется из определения псевдонима по умолчанию.

```javascript
Inputmask.extendAliases({
  'myNum': {
    alias: "numeric",
    placeholder: '',
    allowPlus: false,
    allowMinus: false
  }
});
```

После того, как определено, вы можете вызвать псевдоним с помощью:

```javascript
$(selector).inputmask("myNum");
```

Все обратные вызовы реализованы в виде опций. Это означает, что вы можете установить общие для реализации обратных вызовов путем установки по умолчанию.

```javascript
Inputmask.extendDefaults({
  onKeyValidation: function(key, result){
    if (!result){
      alert('Ваше введенное значение не верно')
    }
  }
});
```

## Методы:
### mask(elems)
Создание маски ввода

```javascript
$(selector).inputmask({ mask: "99-999-99"});
```

или

```javascript
Inputmask({ mask: "99-999-99"}).mask(document.querySelectorAll(selector));
```

или

```javascript
Inputmask("99-999-99").mask(document.querySelectorAll(selector));
```

или

```javascript
var im : new Inputmask("99-999-99");
im.mask(document.querySelectorAll(selector));
```

или

```javascript
Inputmask("99-999-99").mask(selector);
```

### unmaskedvalue
Get the `unmaskedvalue`


```javascript
$(selector).inputmask('unmaskedvalue');
```

или

```javascript
var input = document.getElementById(selector);
if (input.inputmask)
  input.inputmask.unmaskedvalue()
```

#### Value unmasking
Unmask a given value against the mask.

```javascript
var unformattedDate = Inputmask.unmask("23/03/1973", { alias: "dd/mm/yyyy"}); //23031973
```

### удаление
Удаление `inputmask`.

```javascript
$(selector).inputmask('remove');
```

или

```javascript
var input = document.getElementById(selector);
if (input.inputmask)
  input.inputmask.remove()
```

или

```javascript
Inputmask.remove(document.getElementById(selector));
```

### getemptymask
return the default (empty) mask value

```javascript
$(document).ready(function(){
  $("#test").inputmask("999-AAA");
  var initialValue = $("#test").inputmask("getemptymask");  // initialValue  => "___-___"
});
```

### hasMaskedValue
Проверьте маскируется ли возвращаемое значение или нет; В настоящее время только надежно работает при использовании jquery.val функции для извлечения значения

```javascript
$(document).ready(function(){
  function validateMaskedValue(val){}
  function validateValue(val){}

  var val = $("#test").val();
  if ($("#test").inputmask("hasMaskedValue"))
    validateMaskedValue(val);
  else
    validateValue(val);
});
```

### isComplete
Проверяет, осуществлен ли полный ввод значения или нет

```javascript
$(document).ready(function(){
  if ($(selector).inputmask("isComplete")){
    //do something
  }
});
```

### getmetadata
Метаданные фактической маски, представленной в определениях маски может быть получено с помощью вызова getmetadata. Если только маска при условии определения маски будет возвращен getmetadata.

```javascript
$(selector).inputmask("getmetadata");
```

### установка значения
SetValue функциональность, чтобы установить значение для inputmask, как вы могли бы сделать с jQuery.val, но это вызовет внутреннее событие, используемый inputmask всегда, в любом случае. Это особенно полезно при клонировании inputmask с jQuery.clone. Клонирование inputmask не является полностью функциональным клоном. На первом случае (MouseEnter, фокус ...) сотрудник inputmask может обнаружить, если он где клонировали может активировать маскирование. Однако при установке значения с jQuery.val не существует ни одно из событий сработавших в этом случае. SetValue функциональность делает это для вас.

### option(options, noremask)
Get or set an option on an existing inputmask.
The option method is intented for adding extra options like callbacks, etc at a later time to the mask.

When extra options are set the mask is automatically reapplied, unless you pas true for the noremask argument.

Set an option
```javascript
document.querySelector("#CellPhone").inputmask.option({
  onBeforePaste: function (pastedValue, opts) {
    return phoneNumOnPaste(pastedValue, opts);
  }
});
```

```javascript
$("#CellPhone").inputmask("option", {
  onBeforePaste: function (pastedValue, opts) {
    return phoneNumOnPaste(pastedValue, opts);
  }
})
```

### Формат
Вместо того, чтобы маскировать входного элемента также можно использовать для форматирования inputmask для форматирования заданных значений. Подумайте о форматировании значений, чтобы показать в jqGrid или на других элементах затем вводит.

```javascript
var formattedDate = Inputmask.format("2331973", { alias: "dd/mm/yyyy"});
```

### isValid
Проверка заданного значения по отношению к маске.

```javascript
var isValid = Inputmask.isValid("23/03/1973", { alias: "dd/mm/yyyy"});
```

## Опции:
### placeholder
Изменение маски заполнитель.
По умолчанию: "_"

Вместо "_", Вы можете изменить незаполненные символы маски, как вам нравится, просто добавив `placeholder` опцию.<br>
Например, `placeholder: " "` изменится по умолчанию Автоматическое заполнение.

```javascript
$(document).ready(function(){
  $("#date").inputmask("d/m/y",{ "placeholder": "*" });
});
```

или мульти-символьный заполнитель

```javascript
$(document).ready(function(){
  $("#date").inputmask("d/m/y",{ "placeholder": "dd/mm/yyyy" });
});
```

### Необязательный маркер
Определение символов, используемых для указания дополнительного компонента в маске.

```javascript
optionalmarker: { start: "[", end: "]" }
```

### Квантификатор маркер
Определение символов, используемых для обозначения квантора в маске.

```javascript
quantifiermarker: { start: "{", end: "}" }
```

### группа маркеров
Определение символов, используемых для обозначения группы в маске.

```javascript
groupmarker: { start: "(", end: ")" }
```

### генератор маркер
Определение символов, используемых для обозначения генерируемой части в маске.

```javascript
alternatormarker: "|"
```

### escape символ
Определение символов, используемых, чтобы экранировать части в маске.

```javascript
escapeChar: "\\"
```

См. **экранирование специальных символов в маске**

### маска
Маска для использования.

### oncomplete
Выполнить функцию, когда завершен ввод маски

```javascript
$(document).ready(function(){
  $("#date").inputmask("d/m/y",{ "oncomplete": function(){ alert('inputmask complete'); } });
});
```

### onincomplete
Выполнить ф-ю, когда маска является не полной.  Выполняется при фокусе.

```javascript
$(document).ready(function(){
  $("#date").inputmask("d/m/y",{ "onincomplete": function(){ alert('inputmask incomplete'); } });
});
```

### oncleared
Выполнить ф-ю, когда маска очищена.

```javascript
$(document).ready(function(){
  $("#date").inputmask("d/m/y",{ "oncleared": function(){ alert('inputmask cleared'); } });
});
```

### repeat
Повторение ввода. Повторить символ маски ввода  x-раз.

```javascript
$(document).ready(function(){
  $("#number").inputmask({ "mask": "9", "repeat": 10 });  // ~ mask "9999999999"
});
```

### greedy
Toggle to allocate as much possible or the opposite. Non-greedy повторяет функцию.

```javascript
$(document).ready(function(){
  $("#number").inputmask({ "mask": "9", "repeat": 10, "greedy": false });  // ~ mask "9" or mask "99" or ... mask "9999999999"
});
```

С помощью non-greedy опции установите false, и вы можете указать * в качестве повтора.  Это делает повторение бесконечным.

### autoUnmask
Автоматическое снятие маски при извлечении значения.<br>По умолчанию: false.

**При установке этого параметра в true плагин также ожидает, что начальное значение с сервера unmasked.**

### removeMaskOnSubmit
Снимает маску перед отправкой формы.<br>По умолчанию: false

### clearMaskOnLostFocus
Удаляет пустую маску при фокусе или когда не пустой удаляет необязательный косую часть. По умолчанию: true

```javascript
$(document).ready(function(){
  $("#ssn").inputmask("999-99-9999",{placeholder:" ", clearMaskOnLostFocus: true }); //default
});
```

### insertMode
Переключение, чтобы извлечь или переписать ввод.<br>По умолчанию: true.<br>Этот параметр может быть изменен нажатием клавиши Insert.

### clearIncomplete
Очистить неполный ввод при фокусе

```javascript
$(document).ready(function(){
  $("#date").inputmask("d/m/y",{ "clearIncomplete": true } });
});
```

### псевдонимы
Определение псевдонимов.

С помощью псевдонима вы можете определить сложное определение маски и назовите его, используя имя псевдонима. Так что это в основном для упрощения использования ваших масок. Некоторые псевдонимы найдены в расширений: email, currency, decimal, integer, date, datetime, dd/mm/yyyy, etc.

Во-первых, вы должны создать определение псевдонима. Определение псевдонима может содержать опции для маски, пользовательские определения, маска для использования и т.д.

Когда вы передаете в качестве псевдонима, псевдоним сначала решить, а затем другие опции применяются. Таким образом, вы можете назвать псевдоним и передать другую маску, чтобы наносить поверх псевдонима. Это также означает, что вы можете написать псевдонимы, которые "унаследовать" от другого псевдонима.

Некоторые примеры можно найти в jquery.inputmask.xxx.extensions.js

использование:

```javascript
$("#date").inputmask("date");
```

или

```javascript
$("#date").inputmask({ alias: "date"});
```

Можно также вызвать псевдоним и продлить его еще с некоторыми вариантами

```javascript
$("#date").inputmask("date", { "clearIncomplete": true });
```

или

```javascript
$("#date").inputmask({ alias: "date", "clearIncomplete": true });
```

### псевдоним
Псевдоним для использования.

```javascript
$("#date").inputmask({ alias: "email"});
```

### onKeyDown
Обратный вызов для реализации автозаполнения определенных клавиш, например

Аргументы функции: event, buffer, caretPos, opts<br>Возвращаемое значение:

### onBeforeMask
Выполняет перед нанесением масок начальное значение, чтобы обеспечить предварительную обработку исходного значения.

Function arguments: initialValue, opts<br>Function return: processedValue

```javascript
$(selector).inputmask({
  alias: 'phonebe',
  onBeforeMask: function (value, opts) {
    var processedValue = value.replace(/^0/g, "");
    if (processedValue.indexOf("32") > 1 ||     processedValue.indexOf("32") == -1) {
      processedValue = "32" + processedValue;
    }

    return processedValue;
  }
});
```

### onBeforePaste
This callback allows for preprocessing the pasted value before actually handling the value for masking.  This can be usefull for stripping away some characters before processing.

Function arguments: pastedValue, opts<br>Function return: processedValue

```javascript
$(selector).inputmask({
  mask: '9999 9999 9999 9999',
  placeholder: ' ',
  showMaskOnHover: false,
  showMaskOnFocus: false,
  onBeforePaste: function (pastedValue, opts) {
    var processedValue = pastedValue;

    //do something with it

    return processedValue;
  }
});
```

You can also disable pasting a value by returning false in the onBeforePaste call.

Default: Calls the onBeforeMask

### onBeforeWrite
Executes before writing to the masked element

Use this to do some extra processing of the input. This can be usefull when implementing an alias, ex. decimal alias, autofill the digits when leaving the inputfield.

Function arguments: event, buffer, caretPos, opts<br>Function return: command object (see Define custom definitions)

### onUnMask
Executes after unmasking to allow post-processing of the unmaskedvalue.

Function arguments: maskedValue, unmaskedValue<br>Function return: processedValue

```javascript
$(document).ready(function(){
  $("#number").inputmask("decimal", { onUnMask: function(maskedValue, unmaskedValue) {
    //do something with the value
    return unmaskedValue;
  }});
});
```

### showMaskOnFocus
Shows the mask when the input gets focus. (default = true)

```javascript
$(document).ready(function(){
  $("#ssn").inputmask("999-99-9999",{ showMaskOnFocus: true }); //default
});
```

To make sure no mask is visible on focus also set the showMaskOnHover to false.  Otherwise hovering with the mouse will set the mask and will stay on focus.

### showMaskOnHover
Shows the mask when hovering the mouse. (default = true)

```javascript
$(document).ready(function(){
  $("#ssn").inputmask("999-99-9999",{ showMaskOnHover: true }); //default
});
```

### onKeyValidation
Callback function is executed on every keyvalidation with the key & result as parameter.

```javascript
$(document).ready(function(){
  $("#ssn").inputmask("999-99-9999", {
    onKeyValidation: function (key, result) {
      console.log(key + " - " + result);
    }
  });
});
```

### skipOptionalPartCharacter
### showTooltip
Show the current mask definition as a tooltip.

```javascript
$(selector).inputmask({ mask: ["999-999-9999 [x99999]", "+099 99 99 9999[9]-9999"], showTooltip: true });
```

### tooltip
Specify the tooltip to show.  By default the mask definition will be taken.

### numericInput
Numeric input direction.  Keeps the caret at the end.

```javascript
$(document).ready(function(){
  $(selector).inputmask('€ 999.999.999,99', { numericInput: true });    //123456  =>  € ___.__1.234,56
});
```

### rightAlign
Align the input to the right

By setting the rightAlign you can specify to right align an inputmask. This is only applied in combination op the numericInput option or the dir-attribute. Default is true.

```javascript
$(document).ready(function(){
  $(selector).inputmask('decimal', { rightAlign: false });  //disables the right alignment of the decimal input
});
```

### undoOnEscape
Make escape behave like undo. (ctrl-Z)<br>Pressing escape reverts the value to the value before focus.<br>Default: true

### radixPoint (numerics)
Define the radixpoint (decimal separator)<br>Default: ""

### groupSeparator (numerics)
Define the groupseparator<br>Default: ""

### keepStatic
Default: null (~false) Use in combination with the alternator syntax Try to keep the mask static while typing. Decisions to alter the mask will be postponed if possible.

ex. $(selector).inputmask({ mask: ["+55-99-9999-9999", "+55-99-99999-9999", ], keepStatic: true });

typing 1212345123 => should result in +55-12-1234-5123 type extra 4 => switch to +55-12-12345-1234

When passing multiple masks (an array of masks) keepStatic is automatically set to true unless explicitly set through the options.

### positionCaretOnTab
When enabled the caret position is set after the latest valid position on TAB Default: false

### tabThrough
Allows for tabbing through the different parts of the masked field.<br>Default: false

### definitions
### ignorables
### isComplete
With this call-in (hook) you can override the default implementation of the isComplete function.<br>Args => buffer, opts Return => true|false

```javascript
$(selector).inputmask("Regex", {
  regex: "[0-9]*",
  isComplete: function(buffer, opts) {
    return new RegExp(opts.regex).test(buffer.join(''));
  }
});
```

### canClearPosition
Hook to alter the clear behavior in the stripValidPositions<br>Args => maskset, position, lastValidPosition, opts<br>Return => true|false

### postValidation
Hook to postValidate the result from isValid.  Usefull for validating the entry as a whole.  Args => buffer, currentResult, opts<br>Return => true|false

### staticDefinitionSymbol
The staticDefinitionSymbol option is used to indicate that the static entries in the mask can match a certain definition.  Especially usefull with alternators so that static element in the mask can match another alternation.

In the example below we mark the spaces as a possible match for the "i" definition.  By doing so the mask can alternate to the second mask even when we typed already "12 3".

```javascript
Inputmask("(99 99 999999)|(i{+})", {
  definitions: {
    "i": {
      validator: ".",
      cardinality: 1,
      definitionSymbol: "*"
    }
  },
  staticDefinitionSymbol: "*"
}).mask(selector);
```

### nullable
Return nothing when the user hasn't entered anything.
Default: true

### positionCaretOnClick
Positioning of the caret on click.  Options none, lvp (based on the last valid position (default), radixFocus (position caret to radixpoint on initial click)
Default: "lvp"

## General
### set a value and apply mask
this can be done with the traditional jquery.val function (all browsers) or JavaScript value property for browsers which implement lookupGetter or getOwnPropertyDescriptor

```javascript
$(document).ready(function(){
  $("#number").val(12345);

  var number = document.getElementById("number");
  number.value = 12345;
});
```

with the autoUnmaskoption you can change the return of $.fn.val (or value property) to unmaskedvalue or the maskedvalue

```javascript
$(document).ready(function(){
  $('#<%= tbDate.ClientID%>').inputmask({ "mask": "d/m/y", 'autoUnmask' : true});    //  value: 23/03/1973
  alert($('#<%= tbDate.ClientID%>').val());    // shows 23031973     (autoUnmask: true)

  var tbDate = document.getElementById("<%= tbDate.ClientID%>");
  alert(tbDate.value);    // shows 23031973     (autoUnmask: true)
});
```

### escape special mask chars

```javascript
$(document).ready(function(){
  $("#months").inputmask("m \\months");
});
```

### auto-casing inputmask
You can define within a definition to automatically apply some casing on the entry in an input by giving the casing.<br>Casing can be null, "upper", "lower" or "title".

```javascript
Inputmask.extendDefinitions({
  'A': {
    validator: "[A-Za-z]",
    cardinality: 1,
    casing: "upper" //auto uppercasing
  },
  '+': {
    validator: "[A-Za-z\u0410-\u044F\u0401\u04510-9]",
    cardinality: 1,
    casing: "upper"
  }
});
```

Include jquery.inputmask.extensions.js for using the A and # definitions.

```javascript
$(document).ready(function(){
  $("#test").inputmask("999-AAA");    //   => 123abc ===> 123-ABC
});
```

## Supported markup options
### RTL attribute

```html
<input id="test" dir="rtl" />
```

### readonly attribute

```html
<input id="test" readonly="readonly" />
```

### disabled attribute

```html
<input id="test" disabled="disabled" />
```

### maxlength attribute

```html
<input id="test" maxlength="4" />
```

### data-inputmask attribute
You can also apply an inputmask by using the data-inputmask attribute.  In the attribute you specify the options wanted for the inputmask. This gets parsed with $.parseJSON (for the moment), so be sure to use a well-formed json-string without the {}.

```html
<input data-inputmask="'alias': 'date'" />
<input data-inputmask="'mask': '9', 'repeat': 10, 'greedy' : false" />
```

```javascript
$(document).ready(function(){
  $(":input").inputmask();
});
```

### data-inputmask-<option\> attribute
All options can also be passed through data-attributes.

```html
<input data-inputmask-mask="9" data-inputmask-repeat="10" data-inputmask-greedy="false" />
```

```javascript
$(document).ready(function(){
  $(":input").inputmask();
});
```

## jQuery.clone
When cloning a inputmask, the inputmask reactivates on the first event (mouseenter, focus, ...) that happens to the input. If you want to set a value on the cloned inputmask and you want to directly reactivate the masking you have to use $(input).inputmask("setvalue", value)

# jquery.inputmask extensions
## [date & datetime extensions](README_date.md)
## [numeric extensions](README_numeric.md)
## [regex extensions](README_regex.md)
## [phone extensions](README_phone.md)
## [other extensions](README_other.md)

[npm-url]: https://npmjs.org/package/jquery.inputmask
[npm-image]: https://img.shields.io/npm/v/jquery.inputmask.svg
[david-url]: https://david-dm.org/RobinHerbots/jquery.inputmask#info=dependencies
[david-image]: https://img.shields.io/david/RobinHerbots/jquery.inputmask.svg
[david-dev-url]: https://david-dm.org/RobinHerbots/jquery.inputmask#info=devDependencies
[david-dev-image]: https://img.shields.io/david/dev/RobinHerbots/jquery.inputmask.svg
[input-type-ref]: https://html.spec.whatwg.org/multipage/forms.html#do-not-apply
