# Lection 06

### Свойства и методы
Все значения в JavaScript, за исключением null и undefined, содержат набор вспомогательных функций и значений, доступных «через точку».

Такие функции называют «методами», а значения – «свойствами».
 
 ```js
var str = "Привет, мир!";
alert( str.length ); // 12
```

 ```js
var str = "Привет, мир!";
alert( str.toUpperCase() ); // "ПРИВЕТ, МИР!"
```

### Числа 
```js
var n = 12.345;
n.toFixed(2); // "12.35"
```

```js
parseInt('12px'); // 12, ошибка на символе 'p'
parseFloat('12.3.4'); // 12.3, ошибка на второй точке
parseInt('a123'); // NaN”
```

Math.floor - округляет вниз
Math.ceil - округляет вверх
Math.round - округляет до ближайшего целого

```js
Math.floor(3.1);  // 3
Math.ceil(3.1);   // 4
Math.round(3.1);  // 3
```

#### Неточные вычисления
```js
0.1 + 0.2 == 0.3;

9999999999999999
```

Двоичное значение бесконечных дробей хранится только до определенного знака, поэтому возникает неточность.
формат IEEE 754, включая Java, C, PHP, Ruby, Perl.

### Строки
```js
"".charAt(0); // пустая строка
""[0]; // undefined
```

- Содержимое строки в JavaScript нельзя изменять. Нельзя взять символ посередине и заменить его. Как только строка создана – она такая навсегда.
- Методы toLowerCase() и toUpperCase() меняют регистр строки на нижний/верхний.
- Поиск indexOf, lastIndexOf
```js
var str = "Привет мир";

str.indexOf("мир", 2) // 7, поиск начат с позиции 2
```

- slice(start [, end]) Возвращает часть строки от позиции start до, но не включая, позиции end.
```js
"Привет мир".slice(-2); // "ир", от 2 позиции с конца
"Привет мир".slice(1, -1); // "ривет ми"
```

### Объекты как ассоциативные массивы
Объекты в JavaScript сочетают в себе два важных функционала.
Первый – это ассоциативный массив: структура, пригодная для хранения любых данных. 
Второй – языковые возможности для объектно-ориентированного программирования.

Ассоциативный массив – структура данных, в которой можно хранить любые данные в формате ключ-значение.

[Свойства & методы обьекта](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object)

- Создание объектов

```js
var o = new Object();
var o = {};
```

```js
var person = {};
person.name = 'Alec';
person['surname'] = 'Pov';
person.age = 7;
```

- Удаление

```js
delete person.age;
```

- Проверка in и === 
```js
!!person.name;
!!person.lalala;
 "name" in person;
```

- Объявление со свойствами(литеральный)
```js
var person = {
    name: 'Alec',
    surname: 'Pov'
};
```

- Перебор свойств
Порядок перебора соответствует порядку объявления для нечисловых ключей, а числовые – сортируются (в современных браузерах).

```js
for (var key in person) {
    debugger
}
```

- Хранение и копирование
Фундаментальным отличием объектов от примитивов, является их хранение и копирование "по ссылке". В переменной, которой присвоен объект, хранится не сам объект, а "адрес его места в памяти", иными словами – "ссылка" на него.


### Массивы
Массив – разновидность объекта, которая предназначена для хранения пронумерованных значений и предлагает дополнительные методы для удобного манипулирования такой коллекцией.

В качестве ключей выбраны цифры, с дополнительными методами и свойством length.

[Свойства & методы массива](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array)

```js
var a = [];
var a = new Array();
```

```js
var fruits = ["Яблоко", "Апельсин", "Слива"];

fruits.length; // 3
fruits[0]; // Яблоко
fruits[1]; // Апельсин
fruits[2]; // Слива”
```

- Начало массива shift/unshift

```js
var fruits = ["Яблоко", "Апельсин", "Груша"];

fruits.shift(); // удалили Яблоко

fruits.unshift('Мандарин');
```

- Конец массива pop/push
Методы push/pop выполняются быстро, а shift/unshift – медленно.

```js
var fruits = ["Яблоко", "Апельсин", "Груша"];

fruits.pop(); // удалили "Груша"

fruits.push('Мандарин'); // добавили "Мандарин"
```


- splice
Метод splice() изменяет содержимое массива, удаляя существующие элементы и/или добавляя новые.

```js
array.splice(start, deleteCount[, item1[, item2[, ...]]])
```

```js
var myFish = ['ангел', 'клоун', 'мандарин', 'хирург'];

// удаляет 0 элементов с индекса 2 и вставляет элемент 'барабанщик'
var removed = myFish.splice(2, 0, 'барабанщик');
// myFish равен ['ангел', 'клоун', 'барабанщик', 'мандарин', 'хирург']
// removed равен [], никакие элементы не были удалены

// удаляет 1 элемент с индекса 3
removed = myFish.splice(3, 1);
// myFish равен ['ангел', 'клоун', 'барабанщик', 'хирург']
// removed равен ['мандарин']

// удаляет 1 элемент с индекса 2 и вставляет элемент 'телескоп'
removed = myFish.splice(2, 1, 'телескоп');
// myFish равен ['ангел', 'клоун', 'телескоп', 'хирург']
// removed равен ['барабанщик']

// удаляет 2 элемента с индекса 0 и вставляет элементы 'попугай', 'анемон' и 'голубая'
removed = myFish.splice(0, 2, 'попугай', 'анемон', 'голубая');
// myFish равен ['попугай', 'анемон', 'голубая', 'телескоп', 'хирург']
// removed равен ['ангел', 'клоун']

// удаляет 2 элемента с индекса 3
removed = myFish.splice(3, Number.MAX_VALUE);
// myFish равен ['попугай', 'анемон', 'голубая']
// removed равен ['телескоп', 'хирург']
```

- slice
Метод slice() возвращает новый массив, содержащий копию части исходного массива.

```js
arr.slice([begin[, end]])
```

```js
// Пример: цитрусовые среди фруктов
var fruits = ['Банан', 'Апельсин', 'Лимон', 'Яблоко', 'Манго'];
var citrus = fruits.slice(1, 3);

// citrus содержит ['Апельсин', 'Лимон']
```

- join/split

```js
arr.join([separator])
arr.split([separator, limitation])
```

```js
var a = ['Ветер', 'Дождь', 'Огонь'];
a.join();    // 'Ветер,Дождь,Огонь'
a.join('-'); // Ветер-Дождь-Огонь'
```

```js
var str = 'Маша, Петя, Марина, Василий';

str.split(','); // ['Маша', 'Петя', 'Марина', 'Василий']
str.split(',', 2); // ['Маша', 'Петя']
```

- concat

```js
var new_array = old_array.concat(value1[, value2[, ...[, valueN]]])
```

```js
var num1 = [1, 2, 3],
    num2 = [4, 5, 6],
    num3 = [7, 8, 9];

var nums = num1.concat(num2, num3);

console.log(nums); // Результат: [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

- reverse

```js
var myArray = ['один', 'два', 'три'];
myArray.reverse();

console.log(myArray) // ['три', 'два', 'один']
```

- filter

```js
arr.filter(callback[, thisArg])
```

```js
function isBigEnough(value) {
  return value >= 10;
}
var filtered = [12, 5, 8, 130, 44].filter(isBigEnough);
// массив filtered равен [12, 130, 44]
```

- map

```js
var new_array = arr.map(function callback(currentValue, index, array) { 
    // Возвращает элемент для new_array 
}[, thisArg])
```

```js
var numbers = [1, 4, 9];
var doubles = numbers.map(function(num) {
  return num * 2;
});
// теперь doubles равен [2, 8, 18], а numbers всё ещё равен [1, 4, 9]
```

- sort [Быстрая сортировка](http://algolist.manual.ru/sort/quick_sort.php)
Порядок cортировки по умолчанию соответствует порядку кодовых точек Unicode.

```js
arr.sort([compareFunction])
```

 - Если compareFunction(a, b) меньше 0, сортировка поставит a по меньшему индексу, чем b, то есть, a идёт первым.
 - Если compareFunction(a, b) вернёт 0, сортировка оставит a и b неизменными по отношению друг к другу, но отсортирует их по отношению ко всем другим элементам.
 - Если compareFunction(a, b) больше 0, сортировка поставит b по меньшему индексу, чем a.
 - Функция compareFunction(a, b) должна всегда возвращать одинаковое значение для определённой пары элементов a и b. Если будут возвращаться непоследовательные результаты, порядок сортировки будет не определён.
 
 
```js
function compare(a, b) {
  if (a меньше b по некоторому критерию сортировки) {
    return -1;
  }
  if (a больше b по некоторому критерию сортировки) {
    return 1;
  }
  // a должно быть равным b
  return 0;
}
```

```js
var items = [
  { name: 'Edward', value: 21 },
  { name: 'Sharpe', value: 37 },
  { name: 'And', value: 45 },
  { name: 'The', value: -12 },
  { name: 'Magnetic' },
  { name: 'Zeros', value: 37 }
];
items.sort(function (a, b) {
  if (a.name > b.name) {
    return 1;
  }
  if (a.name < b.name) {
    return -1;
  }
  // a должно быть равным b
  return 0;
});
```

- сортировка отображения
```js
// массив для сортировки
var list = ['Дельта', 'альфа', 'ЧАРЛИ', 'браво'];

// временное хранилище позиции и сортируемого значения
var map = list.map(function(e, i) {
  return { index: i, value: e.toLowerCase() };
});

// сортируем карту, содержащую нормализованные значения
map.sort(function(a, b) {
  return +(a.value > b.value) || +(a.value === b.value) - 1;
});

// контейнер для результирующего порядка
var result = map.map(function(e) {
  return list[e.index];
});
```

- Перебор элементов for, forEach()

Метод forEach() выполняет функцию callback один раз для каждого элемента, находящегося в массиве в порядке возрастания. Она не будет вызвана для удалённых или пропущенных элементов массива. Однако, она будет вызвана для элементов, которые присутствуют в массиве и имеют значение undefined.

```js
arr.forEach(function callback(currentValue, index, array) {
    //your iterator
}[, thisArg]);
```

```js
for (var i = 0; i < fruits.length; i++) {
  alert( fruits[i] );
}

fruits.forEach(function (v) {  })
```

- reduce/reduceRight
Используется для последовательной обработки каждого элемента массива с сохранением промежуточного результата.

```js
arr.reduce(callback[, initialValue]);
callback(previousValue, currentItem, index, arr);
```

```js
var sum = [0, 1, 2, 3].reduce(function (a, b) {
  return a + b;
}, 0);
```

```js
var names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice'];

var countedNames = names.reduce(function (allNames, name) { 
  if (name in allNames) {
    allNames[name]++;
  }
  else {
    allNames[name] = 1;
  }
  return allNames;
}, {});
// countedNames is:
// { 'Alice': 2, 'Bob': 1, 'Tiff': 1, 'Bruce': 1 }
```

### Особенности работы length
Встроенные методы для работы с массивом автоматически обновляют его длину length.
Длина length – не количество элементов массива, а последний индекс + 1.

```js
var arr = [];
arr[1000] = true;

alert(arr.length); // 1001
```

При уменьшении length массив укорачивается.

```js
var arr = [1, 2, 3, 4, 5];

arr.length = 2; // укоротить до 2 элементов
alert( arr ); // [1, 2]
```

### Многомерные массивы
Массивы в JavaScript могут содержать в качестве элементов другие массивы. Это можно использовать для создания многомерных массивов, например матриц:

```js
var matrix = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
];

alert( matrix[1][1] ); // 5
```

### Псевдомассив аргументов "arguments"
В JavaScript любая функция может быть вызвана с произвольным количеством аргументов.

Arguments – это не массив Array.

В действительности, это обычный объект, просто ключи числовые и есть length. На этом сходство заканчивается. Никаких особых методов у него нет, и методы массивов он тоже не поддерживает.

```js
function sayHi() {
  for (var i = 0; i < arguments.length; i++) {
    alert( 'Привет, ' + arguments[i] );
  }
}

sayHi('Alice', 'Bob', 'Tiff', 'Bruce', 'Alice');
```

### DOM, BOM и JS

 ![Alt text](./dom.png "DOM, BOM и JS")
 
 - DOM-модель – это внутреннее представление HTML-страницы в виде дерева.
 - Все элементы страницы, включая теги, текст, комментарии, являются узлами DOM.
 - У элементов DOM есть свойства и методы, которые позволяют изменять их.

 ![Alt text](./dom2.png "DOM")
 
```js
var elId = document.getElementById('id'),
    elSel = document.querySelector('selector'),
    elSelAll = document.querySelectorAll('selector');

console.log(elSel.innerHTML);
```

### Заключение

### ДЗ
Сделать страницу сохранения пользователей.
Посетитель заходит на страницу, появляется окно с вопросом - 'Какое кол-во пользователей вы хотите добавить?' минимальное значение 3.
Ровно указанное кол-во раз появляется окно, где через запятую указывается имя, фамилия, имейл, возвраст, профессия.
Указанные данные записываются в массив типа [{name: 'Петя', age: 12 ... }, ..., { ... }], после того как данные введены, появляется окно где можно указать по какому полю сортировать, в итоге вывести результат в отсотированном виде(результат выводить в специальном div c id result).

***Дополнительное задание. Если в поле сортировки ввести 'Sum {имя поля}', то вывести суммарный возвраст если поле возвраст, a для остальных полей все значения введенного поля отсортированные через запятую.

### Справочники
- [Свойства & методы обьекта](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object)
- [Свойства & методы массива](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [Быстрая сортировка](http://algolist.manual.ru/sort/quick_sort.php)