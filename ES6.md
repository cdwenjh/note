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

### 1.数组的解构

```javascript
let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined   解构不成功，变量的值就等于undefined。
z // []
```
### 2.对象的解构
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

### 3.字符串的解构

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

### 4.变量解构赋值用途

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