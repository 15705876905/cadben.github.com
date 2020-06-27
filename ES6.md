##  Let和const
####    let 只在所在代码块中有效
+   var有变量提升，let没有所以必须在声明后使用。
+   暂时性死区，let和const会让声明区块形成一个封闭作用域，变量已经存在，但是不可获取。
+   不允许重复声明

####    ES6的块级作用域
+   块级作用域内声明变量不会影响外层的变量
+   可以代替IIFE 立即执行匿名函数表达式
+   块级作用域内可以声明函数，行为类似let，最好写成函数表达式，且必须在大括号下
+   do表达式，可以返回块级作用的值

####    const 常量
+   作用域和let相同
+   不赋值 会报错

####    顶层对象属性
+   var 和function的全局变量依旧可以window.a，但是let，const，class不属于。
+   global对象，获取顶层对象
    > var getGlobal = function () {
  if (typeof self !== 'undefined') { return self; }
  if (typeof window !== 'undefined') { return window; }
  if (typeof global !== 'undefined') { return global; }
  throw new Error('unable to locate global object');
};  

##  ES6变量的解构赋值
####    模式匹配”，只要等号两边的模式相同，左边的变量就会被赋予对应的值。
####    可以使用默认值，如果是undefined 不会赋值，如果是null会赋值
####    还可以用于对象解析
+   变量必须与属性同名，才能取到正确的值。
    >   var { bar, foo } = { foo: "aaa", bar: "bbb" };
        foo // "aaa"
        bar // "bbb"
+   如果不一样必须使用冒号
    >var { foo: baz } = { foo: 'aaa', bar: 'bbb' };
    baz // "aaa"
+   模式不会赋值
    >var node = {
    loc: {
     start: {
      line: 1,
      column: 5
        }
        }
    };
    var { loc: { start: { line }} } = node;
    line // 1
    loc  // error: loc is undefined
    start // error: start is undefined


##  ES6 字符串的扩展
#### 字符的Unicode表示法 UTF-16存储js内部
+   "\uxxxx"
+   "u{码点}"
+   大于0xFFFF的字符，JS认为他们是两个字符
+   .codePointAt();正确处理4个字节存储的字符
    但是大于0xFFFF的还是得+1，最好用以下操作
    >var s = '𠮷a';
    for (let ch of s) {
         console.log(ch.codePointAt(0).toString(16));
    }
+   字符串的遍历器接口
    for... of
+   .at()可以返回大于0xffff的字符
+   normalize()可以将合成符视为两个字符

####    includes(), startsWith(), endsWith()
+   includes(‘’，5) 返回布尔值，表示是否找到了参数字符串
+   startsWith() 是否在字符串的头部
+   endsWith()是否在尾部
    这三个都可以表示开始搜索的位置

####    repeat()
+   返回一个新的字符串，表示原来字符串重复n次
+   如果是小数，会被取整
+   负数会报错 -0.5不会 
+   如果是字符串参数，则会转化为数字

####    padStart(num,'abc')，padEnd()
+   如果字符串不够指定长度，会在开头和末尾补足指定
+   'xxx'.padStart(2, 'ab') // 'xxx' 长度本身超出
+   'abc'.padStart(10, '0123456789') // '0123456abc' 补齐后超出
+   没有第二个参数，默认用空格
+   可以用于数值补全和字符串格式 '12'.padStart(10, 'YYYY-MM-DD') // "YYYY-MM-12"

##  模板字符串

+   使用反引号，在字符串中嵌入变量和多行字符串。
    >$('#result').append(`
        There are <b>${basket.count}</b> items
    `);
+   .trim()去换行和空格
+   大括号内部可以进行运算
+   还能调用函数

####    标签模板（函数）

+   alert`123` = alert(123)
+   如果 模板字符里面有变量，则会先处理多个参数,变量交替

+   String.raw() 每一个斜杆都被转义


##  正则扩展
+   new RegExg(/abc/ig，'i') 可以有两个参数,对象修饰符ig会被i覆盖
####    y修饰符
+   和g修饰符类似，全局匹配，从上一次匹配成功的下一个位置开始，g只要剩余位置向后有就行，y必须剩余的第一个位置开始
+   如果默认，则从第一个开始，只要第一个不是，直接未找到
+   flags属性，返回修饰符
+   转化 字符串转义：
    > function escapeRegExp(str) {
         return str.replace(/[\-\[\]\/\{\}\(\)\*\+\?\.\\\^\$\|]/g, '\\$&');
    }
+   dotAll模式, 使得.可以匹配任意单个字符
+   先行断言和先行否定断言 x只有在y前面才匹配，/x(?=y)/ 先行否定断言 /x(?!y)/
+   ES7中要加入 后行断言 /(?<=\$)\d+/ 后行否定断言后面 /(?<!y)x/。


##  数值的扩展
+   ES6提供了二进制和八进制数值的新的写法，分别用前缀0b（或0B）和0o（或0O）表示。
+   如果要把0b和0o前缀的字符串数值转化为十进制 用Number()方法；
+   Number.isFinite()判断是否有限
+   Number.isNaN() 是否为Nan
+   Number.parseInt(), Number.parseFloat() 全局方法移植来
+   Number.isInteger() 是否为整数 3和3.0 是一样的 因为整数和浮点数同样的存储
+   Number.EPSILON 极小的常量 用来判断浮点数计算
+   整数范围在-2^53到2^53之间（不含两个端点） JavaScript本身就是
    Number.MAX_SAFE_INTEGER和Number.MIN_SAFE_INTEGER 一个上限 一个下限
+   Number.isSafeInteger()则是用来判断一个整数是否落在这个-2^53和2^53之内。
####    Math对象
+   Math.trunc()去掉小数部分
+   Math.sign方法用来判断一个数到底是正数、负数、还是零。
+   。。。

####    指数运算符
+   2 ** 2 =4

##  数组扩展
####    Array.from()
+   将类似的数组对象 转化成真正的数组
+   var arr1 = [].slice.call(arrayLike); // ['a', 'b', 'c'] ES5
+   let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
+   用于DOM操作返回的NodeList集合
+   z只要有length属性，都可以转
+   可以接受第二个参数，类似map方法

####    Array.of()
+   用于将一组值，转换为数组。

####    实例 copyWithin()
+   指定当前数组位置成员复制到其他位置， 会覆盖

####    数组实例的find()和findIndex() 返回第一个

####    实例 fill(‘n’,start,end)填充满一个数组

####    实例的includes(n，起始位置) 返回是否包含，和indexof 比较 可以识别NaN

##  函数的扩展

####	箭头函数

+ this指向的是调用的对象，而不是使用时所在的对象

+ 如果箭头函数的代码多于一句，就需要大括号把他们括起来

+ 简化回调函数

  ```javascript
  [1,2,3].map(function(x){return x*x});
  [1,2,3].map(x=>x*x);
  ```

+   函数体内的`this`对象，就是定义时所在的对象，而不是使用时所在的对象。因为箭头函数没有this



##	Set和Map

#### Set

+ 初始化Set 

  ```javascript
  var s = new Set([1, 2, 3, 4, 4, 4, 4, 4, 4]);
  [2, 3, 4, 5, 6, 2, 3].map(x => s.add(x));
  ```

+ 属性

  - `Set.prototype.constructor`：构造函数，默认就是`Set`函数。
  - `Set.prototype.size`：返回`Set`实例的成员总数。

+  方法

  - `add(value)`：添加某个值，返回Set结构本身。
  - `delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。
  - `has(value)`：返回一个布尔值，表示该值是否为`Set`的成员。
  - `clear()`：清除所有成员，没有返回值。

+ Array.from可以把Set结构转化为数组

####	遍历操作

+ `keys()`：返回键名的遍历器
+ `values()`：返回键值的遍历器
+ `entries()`：返回键值对的遍历器
+ `forEach()`：使用回调函数遍历每个成员

因为Set中没有key所以，key和value都是一样的

```javascript
// 并集
let union = new Set([...s, ...b]);
// 交集
let intersect = new Set([...s].filter(x => b.has(x)));
// 差集
let difference = new Set([...s].filter(x => !b.has(x)))
```

####	WeakSet

和Set类似，但是只能存放对象



####	Map

map可以把所有的类型都作为键

```javascript
var m = new Map()
            .set(1, 'a')
            .set(2, 'b');
for (let [k, v] of m) {
            console.log(k + ":" + v);
}//等同于 m.entries
```

**（1）Map转为数组**

Map转为数组最方便的方法，就是使用扩展运算符（...）。

```javascript
let myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);
[...myMap]
// [ [ true, 7 ], [ { foo: 3 }, [ 'abc' ] ] ]
```

**（2）数组转为Map**

将数组转入Map构造函数，就可以转为Map。

```javascript
new Map([[true, 7], [{foo: 3}, ['abc']]])
// Map {true => 7, Object {foo: 3} => ['abc']}
```

**（3）Map转为对象**

如果所有Map的键都是字符串，它可以转为对象。

```javascript
function strMapToObj(strMap) {
  let obj = Object.create(null);
  for (let [k,v] of strMap) {
    obj[k] = v;
  }
  return obj;
}

let myMap = new Map().set('yes', true).set('no', false);
strMapToObj(myMap)
// { yes: true, no: false }
```

**（4）对象转为Map**

```javascript
function objToStrMap(obj) {
  let strMap = new Map();
  for (let k of Object.keys(obj)) {
    strMap.set(k, obj[k]);
  }
  return strMap;
}

objToStrMap({yes: true, no: false})
// [ [ 'yes', true ], [ 'no', false ] ]
```

**（5）Map转为JSON**

Map转为JSON要区分两种情况。一种情况是，Map的键名都是字符串，这时可以选择转为对象JSON。

```javascript
function strMapToJson(strMap) {
  return JSON.stringify(strMapToObj(strMap));
}

let myMap = new Map().set('yes', true).set('no', false);
strMapToJson(myMap)
// '{"yes":true,"no":false}'
```

另一种情况是，Map的键名有非字符串，这时可以选择转为数组JSON。

```javascript
function mapToArrayJson(map) {
  return JSON.stringify([...map]);
}

let myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);
mapToArrayJson(myMap)
// '[[true,7],[{"foo":3},["abc"]]]'
```

**（6）JSON转为Map**

JSON转为Map，正常情况下，所有键名都是字符串。

```javascript
function jsonToStrMap(jsonStr) {
  return objToStrMap(JSON.parse(jsonStr));
}

jsonToStrMap('{"yes":true,"no":false}')
// Map {'yes' => true, 'no' => false}
```

但是，有一种特殊情况，整个JSON就是一个数组，且每个数组成员本身，又是一个有两个成员的数组。这时，它可以一一对应地转为Map。这往往是数组转为JSON的逆操作。

```javascript
function jsonToMap(jsonStr) {
  return new Map(JSON.parse(jsonStr));
}

jsonToMap('[[true,7],[{"foo":3},["abc"]]]')
// Map {true => 7, Object {foo: 3} => ['abc']}
```

####	WeakMap

和Map类似，只能用对象作为key，而且key不会被回收



##	Proxy和Reflect

####	Proxy



##	Iterator和for...of循环

+ Iterator相当于创建一个指针对象，调用next方法，从第一个成员直到最后。
+ 当存在Symbol.iterator属性，则就是可遍历的。可以给对象赋值该属性。
+ 三类数据原生具备Iterator接口：数组、类数组对象、Map和Set



##	Generator

####	基本概念

一种异步编程，执行Generator函数，会返回一个遍历器对象。

Generator函数是一个普通函数，但是有两个特征。

+ function`关键字与函数名之间有一个星号；
+ 函数体内部使用`yield`语句，定义不同的内部状态（yield语句在英语里的意思就是“产出”）。

####   yield(暂停标志)

只有next到yield的时候，才会执行方法。能有多个yield，但只有一个return。

使用for...of... 循环以外，扩展运算符（`...`）、解构赋值和`Array.from`方法内部调用的时候，不需要next方法。

####	yield*语句

`yield*`语句，用来在一个Generator函数里面执行另一个Generator函数。



##	Promise对象

####	特点：

+ 三种状态：`Pending`（进行中）、`Resolved`（已完成，又称Fulfilled）和`Rejected`（已失败）
+ 一旦状态改变，就不会再变，任何时候都可以得到这个结果。



##	async函数

`async `函数是 Generator 函数的语法糖。

`async`函数就是将Generator函数的星号（`*`）替换成`async`，将`yield`替换成`await`

Async函数始终返回一个Promise



##	Class

关键词`class`，构造函数`constructor`

类的所有方法都定义在类的`prototype`属性上面。

实例的属性除非显式定义在其本身（即定义在`this`对象上），否则都是定义在原型上（即定义在`class`上）

####	注意

由于p、q的原型都是Point.prototype，所以\__proto__属性相同这意味着两个原型其中一个修改，会影响到所有实例。

```javascript
var p1 = new Point(2,3);
var p2 = new Point(3,2);
p1.__proto__.printName = function () { return 'Oops' };
p1.printName() // "Oops"
p2.printName() // "Oops"
var p3 = new Point(4,2);
p3.printName() // "Oops"
```

####	不存在变量提升，ES5的类可以

####	Class表达式

```
let person = new class {
  constructor(name) {
    this.name = name;
  }
  sayName() {
    console.log(this.name);
  }
}('张三');
person.sayName(); // "张三"
```

####	私有方法

ES6不提供，但是可以变通方法模拟

+ 命名上加以区分，但是外部照样可以调用

+ 将方法移除模块

  ```
  class Widget {
    foo (baz) {
      bar.call(this, baz);
    }
  }
  function bar(baz) {
    return this.snaf = baz;
  }
  ```

+  Symbol值的唯一性

####  this的指向

如果单独把方法提取出来，this会报错

解决方法：

+ 箭头函数

+ 构造方法中绑定this

+ 使用Proxy

  

## Class的继承

Class之间可以通过`extends`关键字实现继承，这比ES5的通过修改原型链实现继承，要清晰和方便很多。必须在子类构造函数中调用super方法，否则建立 实例会出错。

```javascript
class ColorPoint extends Point {}
```

ES5的继承，实质是先创造子类的实例对象`this`，然后再将父类的方法添加到`this`上面（`Parent.apply(this)`）。ES6的继承机制完全不同，实质是先创造父类的实例对象`this`（所以必须先调用`super`方法），然后再用子类的构造函数修改`this`。

####	特殊情况：

+ 继承Object

  ```
  class A extends Object {
  }
  A.__proto__ === Object // true
  A.prototype.__proto__ === Object.prototype // true
  ```

+ 不存在继承

  ```javascript
  class A {
  }
  A.__proto__ === Function.prototype // true
  A.prototype.__proto__ === Object.prototype // true
  ```

+ 继承null

  ```
  class A extends null {
  }
  A.__proto__ === Function.prototype // true
  A.prototype.__proto__ === undefined // true
  ```

#### Object.getPrototypeOf(子类)方法可以用来从子类上获取父类。

#### 实例的__proto__属性

子类实例的__proto__属性的__proto__属性，指向父类实例的__proto__属性。可以修改父类的原型。

```
var p1 = new Point(2, 3);
var p2 = new ColorPoint(2, 3, 'red');
p2.__proto__ === p1.__proto__ // false
p2.__proto__.__proto__ === p1.__proto__ // true
```

####	原生构造函数的继承

####	Class的取值函数（getter）和存值函数（setter）

Class内部可以使用`get`和`set`关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为。

####	Class的静态方法

加上`static`关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。可以被子类继承。

#### Class的静态属性和实例属性

静态属性指的是Class本身的属性，即`Class.propname`，而不是定义在实例对象（`this`）上的属性。

```javascript
class Foo {
}
Foo.prop = 1;
Foo.prop // 1
```

ES6明确规定，Class内部只有静态方法，没有静态属性。

```javascript
// 以下两种写法都无效
class Foo {
  // 写法一
  prop: 2
  // 写法二
  static prop: 2
}
Foo.prop // undefined
```

在ES7中类的实例属性可以用等式，写入类的定义之中。有静态属性。

```javascript
 class Foo {
            static prop = 1;
            static getProp() {
                console.log(this.prop);
            }
    }
    Foo.getProp();
```

##	Decorator修饰器

## 严格模式

ES6 的模块自动采用严格模式，不管你有没有在模块头部加上`"use strict";`。

严格模式主要有以下限制。

- 变量必须声明后再使用
- 函数的参数不能有同名属性，否则报错
- 不能使用`with`语句
- 不能对只读属性赋值，否则报错
- 不能使用前缀0表示八进制数，否则报错
- 不能删除不可删除的属性，否则报错
- 不能删除变量`delete prop`，会报错，只能删除属性`delete global[prop]`
- `eval`不会在它的外层作用域引入变量
- `eval`和`arguments`不能被重新赋值
- `arguments`不会自动反映函数参数的变化
- 不能使用`arguments.callee`
- 不能使用`arguments.caller`
- 禁止`this`指向全局对象
- 不能使用`fn.caller`和`fn.arguments`获取函数调用的堆栈
- 增加了保留字（比如`protected`、`static`和`interface`）

##	module

####	export命令

```javascript
function getCurrentTime() {
    var date = new Date();
    var year = date.getFullYear();
    var month = date.getMonth();
    var day = date.getDay();
    return year + "-" + month + "-" + day;
}
var local = '中国浙江杭州';
class demoClass {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
}
export { local, getCurrentTime, demoClass };
```

####	export default 命令

为模块指定默认输出。

```
export default getCurrentTime;
import deafultdemo from './moduledemo.js'
```

####	import 命令

`import`后面的`from`指定模块文件的位置，可以是相对路径，也可以是绝对路径，`.js`路径可以省略。如果只是模块名，不带有路径，那么必须有配置文件，告诉 JavaScript 引擎该模块的位置。

***(由于`import`是静态执行，所以不能使用表达式和变量，这些只有在运行时才能得到结果的语法结构。import是单例模式)***

####	整体加载

使用*号一次行加载 `import * as demo from './moduledemo.js'`

####	export 与 import 的复合写法

```javascript
export { foo, bar } from 'my_module';
// 等同于
import { foo, bar } from 'my_module';
export { foo, boo};
```

####	ES6模块加载的实质

CommonJS模块加载，是一个值的拷贝，一旦输出模块内部的变化影响不变。

但是module是一个值的引用。

####	游览器中使用ES6模块

```html
<script type="module" src="foo.js"></script>
```

#####	注意：

+ 带有type="module"的 script全部都是异步加载，不会阻塞游览器。
+ 该脚本自动采用严格模块。
+ 该脚本内部的顶层变量，都只在该脚本内部有效，外部不可见。
+ 该脚本内部的顶层的`this`关键字，返回`undefined`，而不是指向`window`

#### 循环加载

