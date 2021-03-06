### Объекты как сущности
 - Методы у объектов
 
 ```js
var user = {
  name: 'Василий'
};

user.sayHi = function() { // присвоили метод после создания объекта
  alert('Привет!');
};

// Вызов метода:
user.sayHi();
```

### Доступ к объекту через this
Для доступа к текущему объекту из метода используется ключевое слово this.

```js
var user = {
  name: 'Василий',

  sayHi: function() {
    alert( this.name );
  }
};

user.sayHi(); // sayHi в контексте user
```

```js
var user = {
  name: 'Василий',

  sayHi: function() {
    alert( user.name );
  }
};

user.sayHi(); // sayHi в контексте user
```
 
```js
var user = {
  name: 'Василий',

  sayHi: function() {
    showName(this); // передать текущий объект в showName
  }
};

function showName(namedObj) {
  alert( namedObj.name );
}

user.sayHi(); // Василий
```

Любая функция может иметь в себе this. Совершенно неважно, объявлена ли она в объекте или отдельно от него.

Значение this называется контекстом вызова и будет определено в момент вызова функции.

```js
function sayHi() {
  alert( this.firstName );
}
```

Если одну и ту же функцию запускать в контексте разных объектов, она будет получать разный this:

```js
var user = { firstName: "Вася" };
var admin = { firstName: "Админ" };

function func() {
  alert( this.firstName );
}

user.f = func;
admin.g = func;

// this равен объекту перед точкой:
user.f(); // Вася
admin.g(); // Админ
admin['g'](); // Админ (не важно, доступ к объекту через точку или квадратные скобки)
```

Значение this не зависит от того, как функция была создана, оно определяется исключительно в момент вызова.

 - Значение this при вызове без контекста

Таково поведение в старом стандарте.
```js
 function func() {
  alert( this ); // выведет [object Window] или [object global]
}

func();
```

А в режиме use strict вместо глобального объекта this будет undefined:

```js
function func() {
  "use strict";
  alert( this ); // выведет undefined (кроме IE9-)
}

func();
```

### Дескрипторы
Основной метод для управления свойствами – Object.defineProperty.

```js
Object.defineProperty(obj, prop, descriptor)
```

 - obj - Объект, в котором объявляется свойство.
 - prop - Имя свойства, которое нужно объявить или модифицировать.
 - descriptor - объект, который описывает поведение свойства.

**Свойства дескриптора:**
 - value – значение свойства, по умолчанию undefined
 - writable – значение свойства можно менять, если true. По умолчанию false.
 - configurable – если true, то свойство можно удалять, а также менять его в дальнейшем при помощи новых вызовов defineProperty. По умолчанию false.
 - enumerable – если true, то свойство просматривается в цикле for..in и методе Object.keys(). По умолчанию false.
 - get – функция, которая возвращает значение свойства. По умолчанию undefined.
 - set – функция, которая записывает значение свойства. По умолчанию undefined.

```js
var user = {};

// 1. простое присваивание
user.name = "Вася";

// 2. указание значения через дескриптор
Object.defineProperty(user, "name", { value: "Вася", configurable: true, writable: true, enumerable: true });
```

```js
"use strict";

var user = {};

Object.defineProperty(user, "name", {
  value: "Вася",
  writable: false, // запретить присвоение "user.name="
  configurable: false // запретить удаление "delete user.name"
});

// Теперь попытаемся изменить это свойство.
// в strict mode присвоение "user.name=" вызовет ошибку
user.name = "Петя";
```

### Преобразование объектов 

**Логическое преобразование**

Любой объект в логическом контексте – true, даже если это пустой массив [] или объект {}.

```js
if ({} && []) {
  alert( "Все объекты - true!" ); // alert сработает
}
```

**Строковое преобразование**

```js
var user = {
  firstName: 'Василий'
};

alert( user ); // [object Object]
```

toString

```js
var user = {

  firstName: 'Василий',

  toString: function() {
    return 'Пользователь ' + this.firstName;
  }
};

alert( user );  // Пользователь Василий
```

**Численное преобразование**

Для численного преобразования объекта используется метод valueOf, а если его нет – то toString:

```js
var room = {
  number: 777,

  valueOf: function() { return this.number; },
  toString: function() { return this.number; }
};

alert( +room );  // 777, вызвался valueOf

delete room.valueOf; // valueOf удалён

alert( +room );  // 777, вызвался toString
```

- В логическом контексте объект – всегда true.
- При строковом преобразовании объекта используется его метод toString. Он должен возвращать примитивное значение, причём не обязательно именно строку.
- Для численного преобразования используется метод valueOf, который также может возвратить любое примитивное значение. У большинства объектов valueOf не работает (возвращает сам объект и потому игнорируется), при этом для численного преобразования используется toString.
 
### Конструктор
Обычный синтаксис {...} позволяет создать один объект. Но зачастую нужно создать много однотипных объектов.

Для этого используют «функции-конструкторы», запуская их при помощи специального оператора new.

Конструктором становится любая функция, вызванная через new.

```js
function Animal(name) {
  this.name = name;
  this.canWalk = true;
}

var animal = new Animal("кот");
```

- Создаётся новый пустой объект.
- Ключевое слово this получает ссылку на этот объект.
- Функция выполняется. Как правило, она модифицирует this, добавляет методы, свойства.
- Возвращается this


```js
function Animal(name) {
  // this = {};

  // в this пишем свойства, методы
  this.name = name;
  this.canWalk = true;

  // return this;
}
```

- Любая функция может быть вызвана с new, при этом она получает новый пустой объект в качестве this, в который она добавляет свойства. Если функция не решит возвратить свой объект, то её результатом будет this.

- Функции, которые предназначены для создания объектов, называются конструкторами. Их названия пишут с большой буквы, чтобы отличать от обычных.

### ООП
На протяжении долгого времени в программировании применялся процедурный подход. При этом программа состоит из функций, вызывающих друг друга.

Объектно-ориентированное программирование (ООП) — это парадигма программирования, которая использует абстракции, чтобы создавать модели, основанные на объектах реального мира. 

ООП использует несколько техник из ранее признанных парадигм, включая модульность, наследование, полиморфизм и инкапсуляция.

ООП представляет программное обеспечение как совокупность взаимодействующих объектов, а не набор функций или просто список команд (как в традиционном представлении). В ООП, каждый объект может получать сообщения, обрабатывать данные, и отправлять сообщения другим объектам. Каждый объект может быть представлен как маленькая независимая машина с отдельной ролью или ответственностью.

ООП способствует большей гибкости и поддерживаемости в программировании, и широко распространена в крупномасштабном программном инжиниринге, благодаря модульности.

ООП – это наука о том, как делать правильную архитектуру. У неё есть свои принципы, например [SOLID](https://ru.wikipedia.org/wiki/SOLID_(%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%BD%D0%BE-%D0%BE%D1%80%D0%B8%D0%B5%D0%BD%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B5_%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5)).

### Терминология
- Класс

Определяет характеристики объекта. Класс является описанием шаблона свойств и методов объекта.

- Полиморфизм

Различные классы могут объявить один и тот же метод или свойство.

- Наследование

Класс может наследовать характеристики от другого класса.

- Инкапсуляция

Отделение и защита внутреннего интерфейса называется.

### ООП в функциональном стиле

```js
function User(name) {

  this.sayHi = function() {
    alert( "Привет, я " + name );
  };

}

var vasya = new User("Вася"); // создали пользователя
vasya.sayHi(); // пользователь умеет говорить "Привет”
```

В JavaScript классы можно организовать по-разному. Говорят, что класс User написан в «функциональном» стиле.

### Прототипное программирование
Прототипное программирование — это модель ООП которая не использует классы, а вместо этого сначала выполняет поведение класса и затем использует его повторно (эквивалент наследования в языках на базе классов), декорируя (или расширяя) существующие объекты прототипы. (Также называемое бесклассовое, прототипно-ориентированное, или экземплярно-ориентированное программирование.)

Объекты в JavaScript можно организовать в цепочки так, чтобы свойство, не найденное в одном объекте, автоматически искалось бы в другом. Связующим звеном выступает специальное свойство __proto__.

```js
var animal = {
  eats: true
};
var rabbit = {
  jumps: true
};

rabbit.__proto__ = animal;

// в rabbit можно найти оба свойства
alert( rabbit.jumps ); // true
alert( rabbit.eats ); // true
```

Объект, на который указывает ссылка __proto__, называется «прототипом». В данном случае получилось, что animal является прототипом для rabbit.

Также говорят, что объект rabbit «прототипно наследует» от animal.

### Функционально - прототипное программирование
- методы в прототипном стиле
- свойства в функциональном стиле

```js
function User(name) {
    this.name = name;
}

User.prototype.sayHi = function() {
    alert( "Привет, я " + this.name );
};

var userAdmin = new User('Admin'),
    userGuest = new User('Guest');

User.prototype.showRole = function() {
     alert("Только ...");
 };
```

- **Достоинства**

Функциональный стиль записывает в каждый объект и свойства и методы, а прототипный – только свойства. Поэтому прототипный стиль – быстрее и экономнее по памяти.

- **Недостатки**

При создании методов через прототип, мы теряем возможность использовать локальные переменные как приватные свойства, у них больше нет общей области видимости с конструктором.

### Наследование 
```js
// 1. Конструктор Animal
function Animal(name) {
  this.name = name;
  this.speed = 0;
}

// 1.1. Методы -- в прототип
Animal.prototype.stop = function() {
  this.speed = 0;
  alert( this.name + ' стоит' );
}

Animal.prototype.run = function(speed) {
  this.speed += speed;
  alert( this.name + ' бежит, скорость ' + this.speed );
};

// 2. Конструктор Rabbit
function Rabbit(name) {
  this.name = name;
  this.speed = 0;
}

// 2.1. Наследование
Rabbit.prototype = Object.create(Animal.prototype);
Rabbit.prototype.constructor = Rabbit;

// 2.2. Методы Rabbit
Rabbit.prototype.jump = function() {
  this.speed++;
  alert( this.name + ' прыгает, скорость ' + this.speed );
}
```

### Заключение

### ДЗ
 Реализуйте класс Worker (Работник), который будет иметь следующие свойства: name (имя), surname (фамилия), rate (ставка за день работы), days (количество отработанных дней). 
 
 Также класс должен иметь метод getSalary(), который будет выводить зарплату работника. Зарплата - это произведение (умножение) ставки rate на количество отработанных дней days.

Вот так должен работать наш класс:

```js
var worker = new Worker('Иван', 'Иванов', 10, 31);

console.log(worker.name); //выведет 'Иван'
console.log(worker.surname); //выведет 'Иванов'
console.log(worker.rate); //выведет 10
console.log(worker.days); //выведет 31
console.log(worker.getSalary()); //выведет 310 - то есть 10*31
```

С помощью нашего класса создайте 3х рабочих и найдите сумму их зарплат.

Создайте еще один класс Director который должен наследовать свойства класса Worker, иметь методы:
  - addWorker(workerName, workerSurname, workerRate, workerDays) добавить работника в список,
  - removeWorker(workerName) удалить работника из списока,
  - getWorker(workerName) получить все данные работника
  - setWorkerRate(workerName, rate) обновить rate  для работника

```js
var director = new Director('Иван', 'Иванов', 100, 31);

director.addWorker('Петя', 'Петров', 10, 31);
director.removeWorker('Петя');
director.getWorker('Петя');
director.setWorkerRate('Петя', 11);
```

Все это должно быть реализовано через форму. Сначала идет поле ввода и кнопка добавить директора. 
Для каждого метода класса директор, своя кнопка с полем ввода(в случае если методу необходимо входное значение). Все результаты 
выводятся в специальном блоке.

Подсказки.

 - document.querySelector 
 - innerHTML 
 - click 
 - addEventListener
 - map
 - push 
 - new
 - prototype
 - p
 - div
 - form 
 - label
 - input type='text'
 - button

### Справочники
- [SOLID](https://ru.wikipedia.org/wiki/SOLID_(%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%BD%D0%BE-%D0%BE%D1%80%D0%B8%D0%B5%D0%BD%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B5_%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5)).