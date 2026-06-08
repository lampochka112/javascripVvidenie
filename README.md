# Основы работы с JavaScript

## Полное руководство для back-end и front-end разработки

Этот документ содержит все фундаментальные знания о JavaScript, необходимые для профессиональной разработки как на клиенте, так и на сервере.

---

## Оглавление

1. [Введение в JavaScript](#введение-в-javascript)
2. [Переменные и типы данных](#переменные-и-типы-данных)
3. [Операторы](#операторы)
4. [Условные конструкции](#условные-конструкции)
5. [Циклы](#циклы)
6. [Функции](#функции)
7. [Объекты](#объекты)
8. [Массивы](#массивы)
9. [Область видимости и замыкания](#область-видимости-и-замыкания)
10. [Ключевое слово `this`](#ключевое-слово-this)
11. [Прототипное наследование](#прототипное-наследование)
12. [Классы (ES6)](#классы-es6)
13. [Асинхронность](#асинхронность)
14. [Обработка ошибок](#обработка-ошибок)
15. [Модули](#модули)
16. [Практические задания](#практические-задания)
17. [Что учить дальше](#что-учить-дальше)
18. [Ресурсы для изучения](#ресурсы-для-изучения)

---

## Введение в JavaScript

### Что такое JavaScript?

JavaScript — это однопоточный, асинхронный, интерпретируемый язык программирования с динамической типизацией. Изначально создан для браузеров, но благодаря Node.js работает и на сервере.

### Ключевые характеристики

| Характеристика | Описание |
|----------------|----------|
| **Динамическая типизация** | Тип переменной определяется значением, а не объявлением |
| **Интерпретируемость** | Не требует компиляции (хотя современные движки JIT-компилируют) |
| **Сборка мусора** | Автоматическое управление памятью |
| **Прототипное наследование** | Объекты наследуются от других объектов |
| **Событийно-ориентированность** | Работа через события и колбэки |
| **Однопоточность + Event Loop** | Выполняет один поток, но асинхронные операции не блокируют |

### История и версии
- **1995** – создание Brendan Eich за 10 дней (Mocha → LiveScript → JavaScript)
- **1997** – стандартизация ECMAScript (ES1)
- **2009** – ES5 (массовое внедрение, `strict mode`, геттеры/сеттеры)
- **2015** – ES6 / ES2015 (революционный релиз: классы, модули, стрелочные функции, промисы, `let`/`const`)
- **с 2016** – ежегодные обновления (ES2016, ES2017, ... ES2024)

---

## Переменные и типы данных

### Три способа объявления переменных

В JavaScript существуют три ключевых слова для создания переменных: `var`, `let`, `const`. Они отличаются областью видимости, возможностью переназначения и «поднятием» (hoisting).

#### Сравнительная таблица

| Характеристика | `var` | `let` | `const` |
|----------------|-------|-------|---------|
| **Область видимости** | Функциональная | Блочная `{ }` | Блочная `{ }` |
| **Можно переназначить** | ✅ | ✅ | ❌ |
| **Можно объявить без инициализации** | ✅ (значение `undefined`) | ✅ (`undefined`) | ❌ (обязательно сразу задать) |
| **Повторное объявление в той же области** | ✅ | ❌ | ❌ |
| **Поднятие (hoisting) с инициализацией `undefined`** | ✅ | ❌ (TDZ) | ❌ (TDZ) |
| **Привязка к глобальному объекту** | ✅ | ❌ | ❌ |

#### Примеры с пояснениями

```javascript
// ==================== var (устаревший, но встречается) ====================
var name = "Alex";
var name = "Boris"; // ✅ повторное объявление разрешено (опасно)

function testVar() {
    if (true) {
        var x = 10; // var игнорирует блок if
    }
    console.log(x); // 10 - доступно во всей функции!
}

// ==================== let (современный стандарт) ====================
let age = 30;
// let age = 31; ❌ SyntaxError: идентификатор уже объявлен

if (true) {
    let blockVar = "я внутри блока";
    console.log(blockVar); // ✅ работает
}
// console.log(blockVar); ❌ ReferenceError

// ==================== const (неизменяемая ссылка) ====================
const BIRTH_YEAR = 1990;
// BIRTH_YEAR = 2000; ❌ TypeError

// ВАЖНО: const не делает объект неизменяемым!
const person = { name: "John", age: 30 };
person.age = 31;        // ✅ изменяем свойство
person.city = "NYC";    // ✅ добавляем новое свойство
// person = {};         // ❌ нельзя переназначить саму переменную

Временная мёртвая зона (Temporal Dead Zone, TDZ)
Переменные, объявленные через let и const, существуют в TDZ от начала блока до момента их объявления. Доступ к ним до объявления вызывает ReferenceError.

javascript
console.log(a); // undefined (var)
var a = 5;

console.log(b); // ReferenceError: Cannot access 'b' before initialization
let b = 10;
Типы данных в JavaScript
JavaScript имеет 8 встроенных типов:

Тип	Описание	Пример
number	Целые и дробные числа (64-битный IEEE 754)	42, 3.14, NaN, Infinity
bigint	Целые числа произвольной длины	9007199254740991n
string	Текст (UTF-16)	"hello", 'world', `template`
boolean	Истина/ложь	true, false
undefined	Значение не присвоено	let x;
null	Намеренное отсутствие значения	let y = null;
symbol	Уникальный идентификатор	Symbol('id')
object	Коллекции и более сложные структуры	{}, [], new Date()
Особенности number
javascript
0.1 + 0.2 === 0.3; // false (из-за погрешности двоичной арифметики)
NaN === NaN;       // false (используйте isNaN() или Number.isNaN())
typeof null;       // "object" (историческая ошибка)
typeof function(){}; // "function" (но function - подтип object)
Преобразование типов
javascript
// Явное преобразование
String(123);      // "123"
Number("456");    // 456
Boolean(0);       // false

// Неявное (опасно!)
"5" + 3;          // "53" (конкатенация)
"5" - 3;          // 2 (вычитание приводит к числу)
if (value) { }    // любой тип приводится к boolean

// Ложные (falsy) значения: false, 0, -0, 0n, "", null, undefined, NaN
// Все остальные - истинные (truthy)
Проверка типа
javascript
typeof 42;                // "number"
typeof "text";            // "string"
typeof true;              // "boolean"
typeof undefined;         // "undefined"
typeof null;              // "object" (ошибка)
typeof Symbol();          // "symbol"
typeof {};                // "object"
typeof [];                // "object"

// Более надёжная проверка
Object.prototype.toString.call([]); // "[object Array]"
Array.isArray([]);                  // true
Операторы
Арифметические операторы
javascript
+   // сложение (или конкатенация строк)
-   // вычитание
*   // умножение
/   // деление
%   // остаток от деления
**  // возведение в степень (ES7)

// Примеры
10 % 3;   // 1
2 ** 4;   // 16
Операторы присваивания
javascript
=    // простое присваивание
+=   // x += y → x = x + y
-=   // x -= y
*=   // x *= y
/=   // x /= y
%=   // x %= y
**=  // x **= y

let x = 5;
x += 3;  // x = 8
Операторы сравнения
Оператор	Описание	Пример (true)
==	равенство с приведением типов	5 == "5"
===	строгое равенство (без приведения)	5 === 5
!=	неравенство с приведением	5 != "6"
!==	строгое неравенство	5 !== "5"
>	больше	10 > 5
<	меньше	5 < 10
>=	больше или равно	5 >= 5
<=	меньше или равно	4 <= 5
Правило: Всегда используйте === и !==, кроме редких осознанных случаев.

javascript
// Странности ==
0 == false;   // true
"" == false;  // true
null == undefined; // true
NaN == NaN;   // false
Логические операторы
javascript
&&  // И (AND) - возвращает первый ложный или последний истинный
||  // ИЛИ (OR) - возвращает первый истинный или последний ложный
!   // НЕ (NOT)

// Примеры короткого замыкания
let name = user && user.name;  // безопасное чтение свойства
let port = config.port || 3000; // значение по умолчанию

// Логическое НЕ
!true;   // false
!!"text"; // true (двойное НЕ приводит к boolean)
Оператор объединения с null (??)
Возвращает правый операнд, если левый равен null или undefined, иначе левый.

javascript
let value = null ?? "default"; // "default"
let value2 = 0 ?? "default";   // 0 (0 не null/undefined)
Тернарный оператор
javascript
let age = 20;
let status = age >= 18 ? "Adult" : "Minor";
// синтаксис: условие ? выражение_если_истина : выражение_если_ложь
Оператор typeof и instanceof
javascript
typeof "hello";           // "string"
typeof null;              // "object"
[] instanceof Array;      // true
{} instanceof Object;     // true
Оператор spread (...) и rest
javascript
// Spread (распаковка)
const arr = [1, 2, 3];
const newArr = [...arr, 4, 5];   // [1,2,3,4,5]

const obj = { a: 1, b: 2 };
const newObj = { ...obj, c: 3 }; // { a:1, b:2, c:3 }

// Rest (сборка)
function sum(...numbers) {
    return numbers.reduce((a,b) => a+b, 0);
}
sum(1,2,3,4); // 10

const [first, second, ...rest] = [10,20,30,40];
// first=10, second=20, rest=[30,40]
Условные конструкции
if / else if / else
javascript
let score = 85;

if (score >= 90) {
    console.log("Отлично");
} else if (score >= 75) {
    console.log("Хорошо");
} else if (score >= 60) {
    console.log("Удовлетворительно");
} else {
    console.log("Неудовлетворительно");
}
switch / case
javascript
let day = 3;
let dayName;

switch (day) {
    case 1:
        dayName = "Понедельник";
        break;
    case 2:
        dayName = "Вторник";
        break;
    case 3:
        dayName = "Среда";
        break;
    default:
        dayName = "Неизвестный день";
}
Использование тернарного оператора для простых условий
javascript
let access = isLoggedIn ? "Доступ разрешён" : "Доступ запрещён";
Циклы
while
javascript
let i = 0;
while (i < 5) {
    console.log(i);
    i++;
}
do...while (выполнится хотя бы раз)
javascript
let j = 0;
do {
    console.log(j);
    j++;
} while (j < 5);
for (классический)
javascript
for (let i = 0; i < 5; i++) {
    console.log(i);
}
for...of (для перебора элементов массива / итерируемых объектов)
javascript
const arr = [10, 20, 30];
for (const value of arr) {
    console.log(value); // 10, 20, 30
}

// Для строк
for (const char of "hello") {
    console.log(char);
}
for...in (для перебора свойств объекта)
javascript
const obj = { a: 1, b: 2, c: 3 };
for (const key in obj) {
    console.log(key, obj[key]); // a 1, b 2, c 3
}

// Внимание: for...in проходит по прототипу, используйте hasOwnProperty
for (const key in obj) {
    if (obj.hasOwnProperty(key)) {
        console.log(key);
    }
}
break и continue
javascript
for (let i = 0; i < 10; i++) {
    if (i === 5) break;      // выход из цикла
    if (i % 2 === 0) continue; // пропуск итерации
    console.log(i); // 1,3
}
Функции
Объявление функций
Function Declaration (объявляется на уровне блока, поднимается)
javascript
function sum(a, b) {
    return a + b;
}
// вызов может быть до объявления
Function Expression (присваивание переменной)
javascript
const multiply = function(a, b) {
    return a * b;
};
// вызов только после объявления
Стрелочные функции (ES6)
javascript
const divide = (a, b) => a / b;

// С телом в блоке
const process = (value) => {
    const result = value * 2;
    return result;
};

// Один параметр без скобок
const square = x => x ** 2;

// Без параметров
const greet = () => "Hello";

// Возврат объекта без return
const createUser = (name, age) => ({ name, age });
Параметры и аргументы
javascript
// Значения по умолчанию
function greet(name = "Гость") {
    console.log(`Привет, ${name}`);
}

// Rest-параметры
function logAll(...args) {
    console.log(args);
}

// Аргументы (устаревший arguments)
function oldSum() {
    let total = 0;
    for (let i = 0; i < arguments.length; i++) {
        total += arguments[i];
    }
    return total;
}
// Но лучше использовать rest: (...numbers)
Возврат значения
Функция без return возвращает undefined

return прерывает выполнение функции

Функции как объекты первого класса
javascript
// Присваивание
const sayHi = () => console.log("Hi");

// Передача как аргумент (колбэк)
function repeat(action, times) {
    for (let i = 0; i < times; i++) {
        action();
    }
}
repeat(() => console.log("Beep"), 3);

// Возврат функции (замыкание)
function createCounter() {
    let count = 0;
    return function() {
        count++;
        return count;
    };
}
const counter = createCounter();
counter(); // 1
counter(); // 2
Чистые функции и побочные эффекты
javascript
// Чистая (pure) - результат зависит только от входных данных, нет побочных эффектов
function pureSum(a, b) {
    return a + b;
}

// Нечистая (impure) - изменяет внешнее состояние
let external = 0;
function impureAdd(x) {
    external += x;
}
Объекты
Создание объектов
javascript
// Литеральная нотация (самый частый способ)
const user = {
    name: "Alice",
    age: 25,
    "favorite-color": "blue", // ключ с дефисом в кавычках
    greet() {                 // метод (сокращённая запись)
        console.log(`Hello, I'm ${this.name}`);
    }
};

// Через new Object()
const obj = new Object();
obj.key = "value";

// Через Object.create()
const prototype = { type: "animal" };
const dog = Object.create(prototype);
dog.name = "Rex";
Доступ к свойствам
javascript
// Точечная нотация
user.name;      // "Alice"

// Скобочная нотация (для динамических ключей или с дефисом)
user["favorite-color"]; // "blue"
const prop = "age";
user[prop];     // 25
Добавление и удаление свойств
javascript
user.email = "alice@example.com";
delete user.age;
Проверка наличия свойства
javascript
"name" in user;           // true
user.hasOwnProperty("name"); // true
user.name !== undefined;  // (не надёжно, если свойство может быть undefined)
Перебор свойств
javascript
for (let key in user) {
    if (user.hasOwnProperty(key)) {
        console.log(key, user[key]);
    }
}

// Современные методы
Object.keys(user);   // ["name", "age", ...]
Object.values(user); // ["Alice", 25, ...]
Object.entries(user); // [["name","Alice"], ["age",25]]
Методы объекта и this
javascript
const calculator = {
    value: 0,
    add(amount) {
        this.value += amount;
        return this; // для цепочки вызовов
    },
    subtract(amount) {
        this.value -= amount;
        return this;
    }
};
calculator.add(5).subtract(2); // цепочка
Геттеры и сеттеры
javascript
const person = {
    firstName: "John",
    lastName: "Doe",
    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    },
    set fullName(name) {
        [this.firstName, this.lastName] = name.split(" ");
    }
};
person.fullName = "Jane Smith";
console.log(person.firstName); // "Jane"
Object.freeze, Object.seal
javascript
const frozen = { x: 1 };
Object.freeze(frozen); // нельзя изменять/добавлять/удалять
frozen.x = 2; // игнорируется (strict mode - ошибка)

const sealed = { y: 2 };
Object.seal(sealed);  // можно менять существующие, нельзя добавлять/удалять
Деструктуризация объектов
javascript
const user = { name: "Bob", age: 28, city: "Moscow" };
const { name, age } = user;
console.log(name, age); // Bob 28

// Переименование и значения по умолчанию
const { name: userName, country = "Unknown" } = user;
Массивы
Создание
javascript
// Литерал
const arr1 = [1, 2, 3];

// Конструктор (редко)
const arr2 = new Array(5);     // пустой массив длины 5
const arr3 = new Array(1,2,3); // [1,2,3]

// Array.of (ES6) - решает проблему одного числа
const arr4 = Array.of(5); // [5]
Основные методы массивов
Добавление / удаление элементов
javascript
const arr = [1, 2, 3];

arr.push(4);        // добавляет в конец → 4, arr = [1,2,3,4]
arr.pop();          // удаляет последний → 4, arr = [1,2,3]
arr.unshift(0);     // добавляет в начало → arr = [0,1,2,3]
arr.shift();        // удаляет первый → 0, arr = [1,2,3]
Срезы и слияние
javascript
const fruits = ["apple", "banana", "cherry", "date"];
fruits.slice(1, 3);    // ["banana", "cherry"] (не изменяет оригинал)
fruits.splice(1, 2, "kiwi", "mango"); // удаляет 2 элемента с индекса 1 и вставляет
// fruits теперь ["apple", "kiwi", "mango", "date"]

const combined = fruits.concat(["pear"]); // новый массив
Поиск
javascript
[1,2,3].indexOf(2);        // 1
[1,2,3].includes(2);       // true
[1,2,3].find(x => x > 1);   // 2 (первый подходящий)
[1,2,3].findIndex(x => x>1); // 1
Итерационные методы (важнейшие)
javascript
const numbers = [1, 2, 3, 4];

// forEach - просто перебор
numbers.forEach(n => console.log(n));

// map - преобразование в новый массив
const doubled = numbers.map(n => n * 2); // [2,4,6,8]

// filter - фильтрация
const evens = numbers.filter(n => n % 2 === 0); // [2,4]

// reduce - свёртка
const sum = numbers.reduce((acc, n) => acc + n, 0); // 10

// some / every
numbers.some(n => n > 3);   // true
numbers.every(n => n > 0);  // true
Сортировка
javascript
const nums = [3, 1, 4, 1, 5];
nums.sort();               // [1,1,3,4,5] (по умолчанию как строки)
nums.sort((a,b) => a - b); // числовая сортировка по возрастанию
nums.sort((a,b) => b - a); // убывание
Деструктуризация массивов
javascript
const rgb = [255, 128, 0];
const [red, green, blue] = rgb;
console.log(red); // 255

// Пропуск элементов
const [, , third] = rgb; // third = 0

// Остаточные элементы (rest)
const [first, ...rest] = [10,20,30,40]; // first=10, rest=[20,30,40]
Spread-оператор с массивами
javascript
const a = [1,2];
const b = [3,4];
const c = [...a, ...b]; // [1,2,3,4]

// Копирование массива
const copy = [...original];
Многомерные массивы
javascript
const matrix = [
    [1,2,3],
    [4,5,6],
    [7,8,9]
];
matrix[1][2]; // 6
Область видимости и замыкания
Глобальная, функциональная, блочная область
javascript
let globalVar = "global";

function outer() {
    let outerVar = "outer";
    
    if (true) {
        let blockVar = "block";
        var functionScopedVar = "functionScoped"; // поднимается на уровень функции
        console.log(globalVar, outerVar, blockVar);
    }
    
    // console.log(blockVar); // ReferenceError
    console.log(functionScopedVar); // "functionScoped"
}
Замыкание (Closure)
Замыкание — это функция, которая «запоминает» окружение, в котором была создана, даже после того, как это окружение исчезло.

javascript
function createCounter(initial) {
    let count = initial;
    return {
        increment() { count++; return count; },
        decrement() { count--; return count; },
        get() { return count; }
    };
}

const counter = createCounter(10);
console.log(counter.increment()); // 11
console.log(counter.decrement()); // 10
// Переменная count недоступна напрямую, но живёт внутри замыкания
Замыкания в циклах (классическая ошибка)
javascript
// Проблема: var не имеет блочной области
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100);
}
// Выведет 3,3,3

// Решение 1: let (создаёт новую привязку на каждой итерации)
for (let i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100);
}
// 0,1,2

// Решение 2: замыкание через IIFE
for (var i = 0; i < 3; i++) {
    (function(j) {
        setTimeout(() => console.log(j), 100);
    })(i);
}
Ключевое слово this
Значение this определяется в момент вызова функции.

Правила определения this
В глобальном контексте – window (браузер) или global (Node.js)

В методе объекта – сам объект

В обычной функции – undefined (в strict mode) или глобальный объект

В стрелочной функции – this из внешнего лексического окружения (не создаёт свой this)

В конструкторе (new) – новый создаваемый объект

Явная привязка – call, apply, bind

javascript
// Примеры
const obj = {
    name: "Object",
    regularFunc: function() { console.log(this.name); },
    arrowFunc: () => { console.log(this.name); } // this = внешний
};

obj.regularFunc(); // "Object"
obj.arrowFunc();   // undefined (или глобальный)

// Привязка через call
function show(prefix) {
    console.log(prefix + this.name);
}
const user = { name: "John" };
show.call(user, "User: "); // "User: John"
show.apply(user, ["User: "]); // аналогично
const bound = show.bind(user, "User: ");
bound(); // "User: John"
Частые ловушки
javascript
const obj = {
    name: "Alice",
    getName: function() {
        return this.name;
    }
};

const getNameRef = obj.getName;
console.log(getNameRef()); // undefined (потеря контекста)

// Исправление: bind
const boundGetName = obj.getName.bind(obj);
console.log(boundGetName()); // "Alice"

// Или стрелочная функция
const obj2 = {
    name: "Bob",
    getName: () => this.name // this = внешний (не работает как метод)
};
Прототипное наследование
Каждый объект в JavaScript имеет внутреннюю ссылку [[Prototype]] (доступна через __proto__ или Object.getPrototypeOf). При обращении к свойству, которого нет в объекте, JS идёт по цепочке прототипов.

Установка прототипа
javascript
const animal = {
    eat() { console.log("Eating"); }
};

const dog = {
    bark() { console.log("Woof"); }
};

// Устанавливаем прототип
Object.setPrototypeOf(dog, animal);
dog.eat(); // "Eating" (наследовано)

// Или через Object.create
const cat = Object.create(animal);
cat.meow = () => console.log("Meow");
cat.eat(); // "Eating"
Свойство constructor и instanceof
javascript
function Person(name) {
    this.name = name;
}
const alice = new Person("Alice");
console.log(alice instanceof Person); // true
console.log(alice.constructor === Person); // true
Прототип функций
У каждой функции есть свойство prototype (не путать с [[Prototype]]). Оно используется только при вызове функции как конструктора (new).

javascript
function User(name) {
    this.name = name;
}
User.prototype.sayHi = function() {
    console.log(`Hi, I'm ${this.name}`);
};

const u = new User("Tom");
u.sayHi(); // "Hi, I'm Tom"
Наследование цепочек
javascript
function Animal(type) {
    this.type = type;
}
Animal.prototype.getType = function() {
    return this.type;
};

function Dog(name) {
    Animal.call(this, "mammal");
    this.name = name;
}
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;
Dog.prototype.bark = function() {
    console.log("Woof");
};

const rex = new Dog("Rex");
rex.getType(); // "mammal"
rex.bark();    // "Woof"
Классы (ES6)
Синтаксический сахар над прототипным наследованием.

Объявление класса
javascript
class Animal {
    constructor(name) {
        this.name = name;
    }

    speak() {
        console.log(`${this.name} makes a sound.`);
    }

    // Статический метод
    static description() {
        return "Animals are living beings";
    }

    // Геттер
    get upperName() {
        return this.name.toUpperCase();
    }

    // Сеттер
    set upperName(val) {
        this.name = val.toLowerCase();
    }
}

const cat = new Animal("Whiskers");
cat.speak(); // "Whiskers makes a sound."
console.log(Animal.description()); // статический метод
Наследование (extends)
javascript
class Dog extends Animal {
    constructor(name, breed) {
        super(name); // вызов конструктора родителя (обязателен)
        this.breed = breed;
    }

    speak() {
        super.speak(); // вызов родительского метода
        console.log(`${this.name} barks.`);
    }

    static description() {
        return super.description() + ", especially dogs";
    }
}

const buddy = new Dog("Buddy", "Golden");
buddy.speak();
// "Buddy makes a sound."
// "Buddy barks."
Приватные поля (#)
javascript
class BankAccount {
    #balance = 0; // приватное поле

    deposit(amount) {
        this.#balance += amount;
    }

    getBalance() {
        return this.#balance;
    }
}

const acc = new BankAccount();
acc.deposit(100);
console.log(acc.getBalance()); // 100
// console.log(acc.#balance); // SyntaxError
Асинхронность
JavaScript — однопоточный, но не блокирующий благодаря событийному циклу (Event Loop).

Callback-функции (основа)
javascript
function fetchData(callback) {
    setTimeout(() => {
        callback("Data received");
    }, 1000);
}

fetchData((data) => {
    console.log(data);
});
// "Data received" через 1 секунду
Проблема: «Callback Hell» (пирамида смерти)

javascript
doA(() => {
    doB(() => {
        doC(() => {
            doD(() => {});
        });
    });
});
Промисы (Promise)
Объект, представляющий результат асинхронной операции (может быть resolved, rejected, pending).

javascript
const promise = new Promise((resolve, reject) => {
    // асинхронная работа
    const success = true;
    if (success) {
        resolve("Value");
    } else {
        reject(new Error("Failed"));
    }
});

promise
    .then(result => console.log(result))
    .catch(error => console.error(error))
    .finally(() => console.log("Done"));
Статические методы Promise
javascript
Promise.all([promise1, promise2]) // ждёт все, reject при любом отказе
Promise.allSettled([...])         // ждёт все, возвращает статусы
Promise.race([...])               // завершится первым
Promise.any([...])                // первый успешный или ошибка, если все упали
Promise.resolve(value)
Promise.reject(error)
Async/Await (синтаксический сахар над промисами)
javascript
async function getUserData(userId) {
    try {
        const response = await fetch(`/api/users/${userId}`);
        if (!response.ok) throw new Error("HTTP error");
        const data = await response.json();
        return data;
    } catch (error) {
        console.error("Failed:", error);
        throw error;
    }
}

// Использование
getUserData(123)
    .then(user => console.log(user))
    .catch(err => console.error(err));
Ключевые моменты:

async функция всегда возвращает Promise

await можно использовать только внутри async (кроме верхнего уровня в модулях)

Ошибки обрабатываются try/catch или через .catch

Event Loop в деталях
Call Stack – выполняет синхронный код

Web APIs / Node APIs – таймеры, запросы, события

Task Queue (MacroTask) – setTimeout, setInterval, I/O

MicroTask Queue – промисы (then, catch, finally), queueMicrotask

Порядок: сначала все микротаски, затем одна макрозадача, затем снова микротаски и т.д.

javascript
console.log("1");
setTimeout(() => console.log("2"), 0);
Promise.resolve().then(() => console.log("3"));
console.log("4");
// Вывод: 1, 4, 3, 2
Обработка ошибок
try...catch...finally
javascript
try {
    // код, который может выбросить ошибку
    let result = riskyOperation();
    console.log(result);
} catch (error) {
    // обработка ошибки
    console.error(error.message);
    console.log(error.stack);
} finally {
    // выполняется всегда (например, закрытие ресурсов)
    console.log("Cleanup");
}
Создание собственных ошибок
javascript
class ValidationError extends Error {
    constructor(message) {
        super(message);
        this.name = "ValidationError";
    }
}

function validateAge(age) {
    if (age < 0) throw new ValidationError("Age cannot be negative");
    if (age > 150) throw new RangeError("Age too high");
    return true;
}
Обработка ошибок в асинхронном коде
javascript
// С промисами
fetch("/data")
    .then(res => res.json())
    .catch(err => console.error("Fetch failed", err));

// С async/await
async function load() {
    try {
        const res = await fetch("/data");
        const data = await res.json();
    } catch (err) {
        console.error(err);
    }
}
Глобальная обработка (браузер/Node.js)
javascript
// Браузер
window.onerror = (message, source, line, col, error) => { /* ... */ };
window.addEventListener("unhandledrejection", event => { /* ... */ });

// Node.js
process.on("uncaughtException", err => { /* ... */ });
process.on("unhandledRejection", (reason, promise) => { /* ... */ });
Модули
ES Modules (современный стандарт)
Файл math.js:

javascript
// Экспорт
export const PI = 3.14159;
export function add(a, b) { return a + b; }
export default function multiply(a, b) { return a * b; }
Файл main.js:

javascript
// Импорт
import multiply, { PI, add as sum } from './math.js';
console.log(multiply(2,3)); // 6
console.log(sum(1,2));      // 3

// Импорт всего
import * as math from './math.js';
math.add(5,5);
CommonJS (Node.js традиционный)
javascript
// exporting.js
module.exports = {
    name: "Module",
    greet() { console.log("Hello"); }
};
// или exports.greet = function() {};

// importing.js
const myModule = require('./exporting');
myModule.greet();
Динамический импорт
javascript
// Ленивая загрузка модуля
button.addEventListener('click', async () => {
    const module = await import('./heavy-module.js');
    module.doSomething();
});
