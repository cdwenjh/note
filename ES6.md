# ES6

## 1.let 和const

let特性：

- 用来声明变量，只在它所在的代码块有效，所以在此块级作用域中不可重复
- 不存在变量提升，声明后必须使用
- 块级作用域中未声明就用 会报错（暂时性死区）
- **typeof 不再是一个百分之百安全的操作**

const特性：

- 具有let的前三个特性
- **const声明一个只读的常量。一旦声明，常量的值就不能改变。const一旦声明变量，就必须立即初始化，不能留到以后赋值。**
- 声明后所指向的内存地址不会变动： 声明的简单类型的数据（数值、字符串、布尔值）等同于常量，复合类型的数据（比如对象、数组）只保存指针，原数据的数据结构是不是可变的，不能控制

## 2.解构赋值
  ES6允许按照一定的模式，从数组和对象中提取值，对变量进行赋值，这被称为解构。

### a.数组的解构

```javascript
let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined   解构不成功，变量的值就等于undefined。
z // []
```
### b.对象的解构
  解构对象时变量名与属性同名才能取到正确的值
```javascript
let { bar, foo } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"

let { baz } = { foo: "aaa", bar: "bbb" };
baz // undefined

```

  当然变量名与属性名不一致时，可以这样写：

```javascript
let { foo: baz } = { foo: 'aaa', bar: 'bbb' };
baz // "aaa"

let obj = { first: 'hello', last: 'world' };
let { first: f, last: l } = obj;
f // 'hello'
l // 'world'
```

  对象的解构赋值的内部机制是先找到同名属性，然后在赋给对应的变量，所以，真正被赋值的是后者而不是前者。对象可以嵌套解构，可以指定默认值

### c.字符串的解构

1)字符串也可以解构赋值。这是因为此时，字符串被转换成了一个类似数组的对象。

2)类似数组的对象都有一个`length`属性，因此还可以对这个属性解构赋值。

```javascript
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"
 
let {length : len} = 'hello';
len // 5
```

### d.变量解构赋值用途

1）交换变量的值。 

```javascript
let x = 1;
let y = 2;
[x,y]=[y,x];
```

2)从函数返回多个值

3)**函数参数的定义，解构赋值可以方便地将一组参数与变量名对应起来。**

```javascript
// 参数是一组有次序的值
function` `f([x, y, z]) { ... }
f([1, 2, 3]);

// 参数是一组无次序的值
function` `f({x, y, z}) { ... }
f({z: 3, y: 2, x: 1});
```

4)提取json数据

```javascript
var jsonData = {
  id: 42,
  status: "OK",
  data: [867, 5309]
};
 
let { id, status, data: number } = jsonData;
 
console.log(id, status, number);
// 42, "OK", [867, 5309]
 
//可以快速提取json中的数据的值。
```

5)遍历 Map 结构

  任何部署了Iterator接口的对象，都可以用`for...of`循环遍历。Map结构原生支持Iterator接口，配合变量的解构赋值，获取键名和键值就非常方便。

```javascript
const map = new Map();
map.set('first', 'hello');
map.set('second', 'world');

for (let [key, value] of map) {
  console.log(key + " is " + value);
}
// first is hello
// second is world

// 获取键名
for (let [key] of map) {
  // ...
}

// 获取键值
for (let [,value] of map) {
  // ...
}
```

## 3.数据类型的扩展

1)字符串的扩展

repeat 方法返回一个新字符串，表示将原字符串重复n次。参数如果是小数则取整，不能小于等于-1，-1（不包含）到1（不包含）视同为0，NAN也是一样，数字型的字符串会自动转换，不是则为“”；

```javascript
'hello'.repeat(2) // "hellohello"
```

padStart()用于头部补全，padEnd()用于尾部补全。

```javascript
'x'.padStart(5, 'ab') // 'ababx'
'x'.padStart(4) // '   x'，第二个参数不填表示为空格

'x'.padEnd(5, 'ab') // 'xabab'
'x'.padEnd(4, 'ab') // 'xaba'

'12'.padStart(10, 'YYYY-MM-DD') // "YYYY-MM-12"
'09-12'.padStart(10, 'YYYY-MM-DD') // "YYYY-09-12" //用来提示字符串格式
```

模板字符串： ``, 变量名：${}

2）函数的扩展

对函数的拓展最大的特点就是能为函数的参数指定默认值。

```javascript
function Point(x = 0, y = 0) {
  this.x = x;
  this.y = y;
}

const p = new Point();
p // { x: 0, y: 0 }
```

还有一个重大更新：箭头函数。

写法：函数名=(形参)=>{……}     当函数体中只有一个表达式时，{}和return可以省略，当函数体中形参只有一个时，()可以省略。

特点：箭头函数中的this始终指向箭头函数定义时的离this最近的一个函数，如果没有最近的函数就指向window。

```javascript
var f = () => 5;
// 等同于
var f = function () { return 5 };

var sum = (num1, num2) => num1 + num2;
// 等同于
var sum = function(num1, num2) {
  return num1 + num2;
};
```

3）扩展运算符

三个点（...），主要用于函数调用，比如将一个数组，变为参数序列。

```javascript
function push(array, ...items) {
  array.push(...items);
}

function add(x, y) {
  return x + y;
}

const numbers = [4, 38];
add(...numbers) // 42

// ES5 的写法
function f(x, y, z) {
  // ...
}
var args = [0, 1, 2];
f.apply(null, args);

// ES6的写法
function f(x, y, z) {
  // ...
}
let args = [0, 1, 2];
f(...args);

//复制数组
const a1 = [1, 2];
// 写法一
const a2 = [...a1];
// 写法二
const [...a2] = a1;
```

## 4.Set和Map数据结构

Set 同Java中的Set一样的，不能有重复的数据，内置了增-add()、删-delete()、判断-has()、清空-clear()、转换数组-Array.from().；另外还有一些遍历的方法，但是在遍历中不能改变原Set结构。

```javascript
let set = new Set(['red', 'green', 'blue']);
let arr = [...set];
// ['red', 'green', 'blue']

//  去除数组的重复成员。
let arr = [3, 5, 2, 2, 5, 5];
let unique = [...new Set(arr)];
// [3, 5, 2]

// 改变原Set结构思路
// 方法一
let set = new Set([1, 2, 3]);
set = new Set([...set].map(val => val * 2));
// set的值是2, 4, 6

// 方法二
let set = new Set([1, 2, 3]);
set = new Set(Array.from(set, val => val * 2));
// set的值是2, 4, 6
```

WeakSet结构与Set类似，只不过成员只能是对象，而且其中的队形都是弱引用，适合临时存放一组对象，不可遍历。

Map用法同Java中也差不多，键值对结构，可以把对象当做键，但是只有对同一个对象的引用，Map结构才会视为同一个键。

```javascript
const map = new Map();

map.set(['a'], 555);
map.get(['a']) // undefined 表面是同一个键，实际内存地址不一样

const k1 = ['b'];
const k2 = ['b'];

map
.set(k1, 111)
.set(k2, 222);

map.get(k1) // 111
map.get(k2) // 222
```

## 5.Symbol

  Symbol是一种新的原始数据类型，值是通过Symbol()函数生成，可以设置多个，属性名属于Symbol类型的都是独一无二的，即使继承也不会传递，其值也不能用于运算。

```javascript
let mySymbol = Symbol();

// 第一种写法
let a = {};
a[mySymbol] = 'Hello!';

// 第二种写法
let a = {
  [mySymbol]: 'Hello!'
};

// 第三种写法
let a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });

// 以上写法都得到同样结果
```

Symbol还可以通过Symbol.for()生成,但与Symbol()有很大区别。

> Symbol.for()与Symbol()这两种写法，都会生成新的 Symbol。它们的区别是，前者会被登记在全局环境中供搜索，后者不会。Symbol.for()不会每次调用就返回一个新的 Symbol 类型的值，而是会先检查给定的key是否已经存在，如果不存在才会新建一个值。比如，如果你调用Symbol.for("cat")30 次，每次都会返回同一个 Symbol 值，但是调用Symbol("cat")30 次，会返回 30 个不同的 Symbol 值。

```javascript
let s1 = Symbol.for('foo');
let s2 = Symbol.for('foo');

s1 === s2 // true
```

## 6.Reflect

Reflect有多用处，比如让object操作都变成函数行为：

```javascript
delete obj[name]；
//改为
Reflect.deleteProperty(obj, name)；
```

将Object 对象的一些明显属于语言内部的方法放到Reflect对象上；

```javascript
// 老写法
try {
  Object.defineProperty(target, property, attributes);
  // success
} catch (e) {
  // failure
}

// 新写法
if (Reflect.defineProperty(target, property, attributes)) {
  // success
} else {
  // failure
}
```

## 7.**Promise对象**

Promise是异步编程的一种解决方案，将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。

它有三种状态，分别是pending-进行中、resolved-已完成、rejected-已失败。

Promise 构造函数包含一个参数和一个带有 resolve（解析）和 reject（拒绝）两个参数的回调。在回调中执行一些操作（例如异步），如果一切都正常，则调用 resolve，否则调用 reject。对于已经实例化过的 promise 对象可以调用 promise.then() 方法，传递 resolve 和 reject 方法作为回调。then()方法接收两个参数：onResolve和onReject，分别代表当前 promise 对象在成功或失败时。

```javascript
var promise = new Promise((resolve, reject) => {
    var success = true;
    if (success) {
        resolve('成功');
    } else {
        reject('失败');
    }
}).then(
    (data) => { console.log(data)},
    (data) => { console.log(data)}
）
```

promise的执行过程：

```javascript
setTimeout(function() {
    console.log(0);
}, 0);
var promise = new Promise((resolve, reject) => {
    console.log(1);
    setTimeout(function () {
        var success = true;
        if (success) {
            resolve('成功');
        } else {
            reject('失败');
        }
    },2000);
}).then(
    (data) => { console.log(data)},
    (data) => { console.log(data)}
);
console.log(promise);	//<pending> 进行中
setTimeout(function () {
    console.log(promise);	//<resolved> 已完成
},2500);
console.log(2);
 
//1
//Promise {<pending>}
//2
//0
//成功
//Promise {<resolved>: undefined}
```

## 8.Generator函数

Generator函数从语法上可以理解为一个状态机，封装了多个内部状态；执行Generator函数会返回一个遍历器对象，可以依次遍历Generator函数内部的每一个状态。
 Generator函数两个特征;function关键字和函数名之间有个*号，函数体内部使用yield表达式，定义不同的内部状态。

```javascript
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

var hw = helloWorldGenerator();
hw.next()
// { value: 'hello', done: false }

hw.next()
// { value: 'world', done: false }
```

yield表达式就是暂停标志。yield表达式后面的表达式，只有当调用next方法、内部指针指向该语句时才会执行，当处于for...of循环体内时，循环会自动调用。
 当一个对象不具备 Itreator接口是，无法使员工for...of遍历，这是可以用Genertaorg函数为这个对象加上遍历器接口；或者把Generator函数添加到对象的Symbol,iterator属性上。

```javascript
function* objectEntries() {
  let propKeys = Object.keys(this);

  for (let propKey of propKeys) {
    yield [propKey, this[propKey]];
  }
}

let jane = { first: 'Jane', last: 'Doe' };

jane[Symbol.iterator] = objectEntries;

for (let [key, value] of jane) {
  console.log(`${key}: ${value}`);
}
// first: Jane
// last: Doe
```