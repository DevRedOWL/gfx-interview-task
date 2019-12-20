# gfx-interview-task
Ссылка на штуковину с прямоугольниками (пока не готово): https://codepen.io/TorTik59/pen/vYExxvO

## Задание 0
```js
// Пояснение
if(x == NaN) x = 0; // NaN не работает с операторами равенства

// Решение
isNaN(x) ? (x = 0) : x; // Для таких случаев создан специальный метод
```

## Задание 1
```js
// Пояснение
var x = 0.3 - 0.1; // Из за способа представления чисел с плавающей точкой в памяти в подобных рассчетах возникает погрешность
if(x == 0.2) x++; // Инкремент никогда не произойдет потому что .2 + .1 = 0.30000000000000004

// Решение
var kostil = 10; // К сожалению
var x = ((0.3 * kostil) - (0.1 * kostil)) / kostil;
(x == 0.2) ? ++x : x;
```

## Задание 2
```js
// Пояснение
var a = [0, 1, 2, 3];
for(var i = 0; i < a.lengh; i++) // Цикл for не начнет работу из за опечатки в length (a.lengh это null, нечисло)
	if(a[i] == 2)                   
		a.splice(i, 1); // Таким образом мы удаляем элемент из массива и последующие индексы сдвигаются, а итератор не изменяется и цикл перескакивает через следующий элемент, в таком состоянии алгоритма каждая вторая двойка удалена не будет

// Решение
var a = [0, 1, 2, 3];
for(var i = 0; i < a.length; i++) // Исправляем опечатку
	if(a[i] == 2)      
		a.splice(i--, 1); // Декрементируем i, чтобы после смещения индексов массива влево проверить элемент, который встанет на место удаленного
```

## Задание 3
```js
// Решение с пояснениями
class A {
  	// Для начала будем использовать инкапсуляцию и отделим поле от свойства 
	constructor(x = 0, y = 0, z = 0) {
  		this._x = x;
		this._y = y; // Тут была опечатка не y, а x
		this._z = z;
	}
  
  	// Быть там может что угодно, а возвращает пусть числа
  	get x() { return isNaN(this._x) ? 0 : this._x; };
  	get y() { return isNaN(this._y) ? 0 : this._y; };
  	get z() { return isNaN(this._z) ? 0 : this._z; };  
}

class B extends A {
	constructor(x = 0, y = 0, z = 0, w = 0) {
		super(x, y, z); // Конструктор класса потомка должен сначала вызывать super
		this._w = w;
	}	

  	// По той же причине, что и в родительском классе, создаем геттер
  	get w() { return isNaN(this._w) ? 0 : this._w; };
}

// Отлад очка
let a = new B(1,'f',3,5);
console.log(a.x+' '+a.y+' '+a.z+' '+a.w);
```

## Задание 4
``` js 
// Пояснение сразу в решении
class Game {
	start() { console.log('New game!'); } // Для отладки
	clear() { }
	restart() {
		this.clear();
		setTimeout(() => { // Просто синтаксис другой, в ES6 это через стрелочные функции
		    this.start();
		}, 16);
	}
}

let game = new Game(); // Чтобы объект был виден только внутри блока, рекомендуется юзать let вместо var
game.restart();
```

## Задание 5
``` js
// Оптимизированное решение
function load(url, callback) {
  	// Так понял тут что то типа имитации задержки? Окей.
	if(Math.random() > 0.5) 
    		setTimeout(callback(url), 100); // После сотни была точка с запятой а в колбеке не хватало скобок
	else callback(url);
}

var urls = []; // Немножечко оптимизируем заполнение
for(var i = 0; i < 4; i++) urls.push("http://www.w"+i+".org")

for(var i = 0; i < urls.length; i++) {
	load(urls[i], (data) => { console.log(data + ' loaded'); }); // Для отладки выведем урл
	if(i == urls.length-1) console.log('Finaly loaded!'); // Эта строчка должна была быть не вложенной в колбек	
}
```
