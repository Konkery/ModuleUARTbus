# ModuleBaseUARTbus
<p align="center">
  <img src="./res/logo.png" width="400" title="hover text">
</p>

-----------------

# Лицензия
////

# Описание
<div style = "font-family: 'Open Sans', sans-serif; font-size: 16px; color: #555">

Модуль предназначен для менеджмента UART шин и реализует базовые операции по созданию общего для проекта хранилища объектов UART шины в среде Espruino. Модуль динамически создаваёт и добавляет в контейнер новый объект UART шины и предоставляет прикладным классам экземпляры объектов, а также хранит информацию о том - занята данная, конкретная шина или нет. Модуль хранит экземпляры предопределенных в Espruino UART шин (Serial1, Serial2, Serial3, Serial4, Serial5, Serial6), а также создает soft шины UART. При создании возвращается объект типа UART шина. Модуль является неотъемлемой частью фреймворка EcoLite. Модуль работает в режиме синглтон. Для корректной работы фреймворка необходимо создать глобальный объект с именем UARTbus. Модуль имеет следующие архитектурные решения фреймворка EcoLite:
- при проверке валидности данных использует ошибку класса [Error](https://github.com/Konkery/ModuleAppError/blob/main/README.md);
- при проверке переменной на целочисленное использует класс [NumIs](https://github.com/Konkery/ModuleAppMath/blob/main/README.md).

### Конструктор
Конструктор не принимает никаких значений, и при создании объекта класса произойдёт разовое занесение в массив существующих в системе шин. Массив содержит объекты с двумя полями: непосредственно объект шины и идентификатор *true/false* об её использовании в системе. Пример объекта массива:
```js
this._UARTBus[] = {
    IDBus: UART{};
    Used: true;
}
```

### Поля
- <mark style="background-color: lightblue">_UARTbus</mark> - массив-контейнер с UART шинами;
- <mark style="background-color: lightblue">_pattern</mark> - строка-ключ, для всех объектов шин;
- <mark style="background-color: lightblue">_indexBus</mark> - индекс софтверной шины. Начальный - 10, конкатенацией с полем _pattern составляет имя нового объекта-шины.

### Методы
- <mark style="background-color: lightblue">Init()</mark> - заносит в массив-контейнер существующие в системе UART шины, запускается в конструкторе;
- <mark style="background-color: lightblue">AddBus(_opts)</mark> - создаёт новую софтверную шину и заносит её в массив-контейнер.
Принимает объект *_opts*, содержащий пины и битрейт создаваемой шины. Метод проводит проверку валидности данных. Пример объектра *_opts*:
```js
let _opts = {
    rx: B7;
    tx: B6;
    baud: 115200;
}
```
Метод возвращает объект, содержащий имя шины и объект UART. Пример:
```js
return {
    NameBus: 'UART10';
    IDBus: UART{};
}
```

### Примеры
Фрагмент кода для создание софтверной шины. Предполагается, что все необходимые модули уже загружены в систему:
```js
//Подключение необходимых модулей
const ClassUARTBus = require('ClassBaseUARTBus.min.js');
const err = require('ModuleAppError.min.js');
const NumIs = require('ModuleAppMath.min.js');
     NumIs.is(); //добавить функцию проверки целочисленных чисел в Number

//Создание UART шины
let UARTbus = new ClassUARTBus();
let bus = UARTbus.AddBus({rx: P5, tx: P4, baud: 115200}).IDbus;

console.log(UARTbus);
```
Вывод созданнного объекта в консоль:
<p align="left">
  <img src="./res/output.png" title="hover text">
</p>

# Зависимости
- <mark style="background-color: lightblue">[**ModuleAppError**](https://github.com/Konkery/ModuleAppError/blob/main/README.md)</mark>
- <mark style="background-color: lightblue">[**ModuleAppMath**](https://github.com/Konkery/ModuleAppMath/blob/main/README.md)</mark>
</div>
