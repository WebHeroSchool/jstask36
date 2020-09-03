# Стиль кода JavaScript

- Объявление переменных
- Объекты
- Массивы
- Строки
- Функции
- Классы и конструкторы
- Свойства
- Пробелы
- Запятые
- Соглашение об именовании
## Объявление переменных
 Используйте const для объявления переменных; избегайте var.

### Пример
```
// плохо
var a = 1;
var b = 2;

// хорошо
const a = 1;
const b = 2;
```
Если вам необходимо переопределять значения, то используйте let вместо var.
### Пример
```
// плохо
var count = 1;
if (true) {
  count += 1;
}

// хорошо, используйте let.
let count = 1;
if (true) {
  count += 1;
}
```
## Объекты
Для создания объекта используйте литеральную нотацию.

### Пример
```
// плохо
const item = new Object();

// хорошо
const item = {};
```
Используйте сокращённую запись метода объекта.
### Пример
```
// плохо
const atom = {
  value: 1,

  addValue: function (value) {
    return atom.value + value;
  },
};

// хорошо
const atom = {
  value: 1,

  addValue(value) {
    return atom.value + value;
  },
};
```
Только недопустимые идентификаторы помещаются в кавычки.

### Пример
```
// плохо
const bad = {
  'foo': 3,
  'bar': 4,
  'data-blah': 5,
};

// хорошо
const good = {
  foo: 3,
  bar: 4,
  'data-blah': 5,
};
```
## Массивы
Для создания массивов стоит использовать квадратные скобки. Создавать масиивы через конструктор new Array не нужно.

### Пример
```
var elements = [];
```
Для копирования массивов используйте оператор расширения ....

### Пример
```
// плохо
const len = items.length;
const itemsCopy = [];
let i;

for (i = 0; i < len; i += 1) {
  itemsCopy[i] = items[i];
}

// хорошо
const itemsCopy = [...items];
```
Если массив располагается на нескольких строках, то используйте разрывы строк после открытия и перед закрытием скобок.

### Пример
```
// плохо
const arr = [
  [0, 1], [2, 3], [4, 5],
];

const objectInArray = [{
  id: 1,
}, {
  id: 2,
}];

const numberInArray = [
  1, 2,
];


// хорошо
const arr = [[0, 1], [2, 3], [4, 5]];

const objectInArray = [
  {
    id: 1,
  },
  {
    id: 2,
  },
];

const numberInArray = [
  1,
  2,
];
```
## Строки
Используйте одинарные кавычки '' для строк.

### Пример
```
// плохо
const name = "Capt. Janeway";

// плохо - литерал шаблонной строки должен содержать интерполяцию или переводы строк
const name = `Capt. Janeway`;

// хорошо
const name = 'Capt. Janeway';
```
При создании строки программным путём используйте шаблонные строки вместо конкатенации.
### Пример
```
// плохо
function sayHi(name) {
  return 'How are you, ' + name + '?';
}

// плохо
function sayHi(name) {
  return ['How are you, ', name, '?'].join();
}

// плохо
function sayHi(name) {
  return `How are you, ${ name }?`;
}

// хорошо
function sayHi(name) {
  return `How are you, ${name}?`;
}
```
Не используйте в строках необязательные экранирующие символы.

### Пример
```
// плохо
const foo = '\'this\' \i\s \"quoted\"';

// хорошо
const foo = '\'this\' is "quoted"';
const foo = `my name is '${name}'`;
```
## Функции
Оборачивайте в скобки немедленно вызываемые функции.

### Пример
```
// Немедленно вызываемая функция
(function () {
  console.log('Welcome to the Internet. Please follow me.');
}());
```
Никогда не называйте параметр arguments. Он будет иметь приоритет над объектом arguments, который доступен для каждой функции.

### Пример
```
// плохо
function foo(name, options, arguments) {
  // ...
}

// хорошо
function foo(name, options, args) {
  // ...
}
```
Используйте синтаксис записи аргументов по умолчанию, а не изменяйте аргументы функции.

### Пример
```
// очень плохо
function handleThings(opts) {
  // Нет! Мы не должны изменять аргументы функции.
  // Плохо вдвойне: если переменная opts будет ложной,
  // то ей присвоится пустой объект, а не то что вы хотели.
  // Это приведёт к коварным ошибкам.
  opts = opts || {};
  // ...
}

// всё ещё плохо
function handleThings(opts) {
  if (opts === void 0) {
    opts = {};
  }
  // ...
}

// хорошо
function handleThings(opts = {}) {
  // ...
}
```
Никогда не переназначайте параметры.

### Пример
```
// плохо
function f1(a) {
  a = 1;
  // ...
}

function f2(a) {
  if (!a) { a = 1; }
  // ...
}

// хорошо
function f3(a) {
  const b = a || 1;
  // ...
}

function f4(a = 1) {
  // ...
}
```
## Классы и конструкторы
Всегда используйте class. Избегайте прямых манипуляций с prototype.

### Пример
```
// плохо
function Queue(contents = []) {
  this.queue = [...contents];
}
Queue.prototype.pop = function () {
  const value = this.queue[0];
  this.queue.splice(0, 1);
  return value;
};

// хорошо
class Queue {
  constructor(contents = []) {
    this.queue = [...contents];
  }
  pop() {
    const value = this.queue[0];
    this.queue.splice(0, 1);
    return value;
  }
}
```
У классов есть конструктор по умолчанию, если он не задан явно. Можно опустить пустой конструктор или конструктор, который только делегирует выполнение родительскому классу.

### Пример
```
// плохо
class Jedi {
  constructor() {}

  getName() {
    return this.name;
  }
}

// плохо
class Rey extends Jedi {
  constructor(...args) {
    super(...args);
  }
}

// хорошо
class Rey extends Jedi {
  constructor(...args) {
    super(...args);
    this.name = 'Rey';
  }
}
```
Избегайте дублирующих членов класса.

### Пример
```
// плохо
class Foo {
  bar() { return 1; }
  bar() { return 2; }
}

// хорошо
class Foo {
  bar() { return 1; }
}

// хорошо
class Foo {
  bar() { return 2; }
}
```
## Свойства
Используйте точечную нотацию для доступа к свойствам.

### Пример
```
const luke = {
  jedi: true,
  age: 28,
};

// плохо
const isJedi = luke['jedi'];

// хорошо
const isJedi = luke.jedi;
```
Используйте скобочную нотацию [], когда название свойства хранится в переменной.

### Пример
```
const luke = {
  jedi: true,
  age: 28,
};

function getProp(prop) {
  return luke[prop];
}

const isJedi = getProp('jedi');
```
## Пробелы
Используйте мягкую табуляцию (символ пробела) шириной в 2 пробела.

### Пример
```
// плохо
function foo() {
∙∙∙∙let name;
}

// плохо
function bar() {
∙let name;
}

// хорошо
function baz() {
∙∙let name;
}
```
Разделяйте операторы пробелами.

### Пример
```
// плохо
const x=y+5;

// хорошо
const x = y + 5;
```
## Запятые
Не начинайте строку с запятой.

### Пример
```
// плохо
const story = [
    once
  , upon
  , aTime
];

// хорошо
const story = [
  once,
  upon,
  aTime,
];

// плохо
const hero = {
    firstName: 'Ada'
  , lastName: 'Lovelace'
  , birthYear: 1815
  , superPower: 'computers'
};

// хорошо
const hero = {
  firstName: 'Ada',
  lastName: 'Lovelace',
  birthYear: 1815,
  superPower: 'computers',
};
```
## Соглашение об именовании
Избегайте названий из одной буквы. Имя должно быть наглядным.

### Пример
```
// плохо
function q() {
  // ...
}

// хорошо
function query() {
  // ...
}
```
Используйте camelCase для именования объектов, функций и экземпляров.

### Пример
```
// плохо
const OBJEcttsssss = {};
const this_is_my_object = {};
function c() {}

// хорошо
const thisIsMyObject = {};
function thisIsMyFunction() {}
```