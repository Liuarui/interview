### ES6新特性一览

- `let`和`const`
- 暂时性死区
- 解构赋值
- 字符串的`unicode`表示
- 模板字符串
- 对象简写属性
- 函数的默认参数
- 函数的剩余参数（rest参数）
- spread操作符
- 箭头函数
- 尾调用优化
- 遍历器
- for...of...
- Symbol
- Map和Set
- class
- Promise
- Generator
- async await
- Proxy和Reflect
- 二进制数组
- 模块
- Math、Number、String、Object、RegExp扩展

### let和const的异同

**相同点**

- `let`和`const`声明的变量，都是块级作用域，都只在其所在的`代码块`内有效
- `let`和`const`声明的变量，都不存在变量提升
- `let`和`const`声明的变量，都存在临时性死区
- `let`和`const`都不允许在通过一个作用于内重复声明同一个变量
- `let`和`const`都不允许在函数内对参数重新声明
- `let`和`const`声明的变量，都不是顶层对象的属性

**不同点**

- `let`声明的是变量，声明后，可以在任意时刻赋值，修改
- `const`声明的是常量，声明后必须立刻赋值，且以后不允许修改

### let和var的异同

**相同点**

- `let`和`var`都是声明一个变量
- `let`和`var`声明的变量，都是可以在声明后，任意时刻赋值，修改

**不同点**

- `let`无变量提升，`var`有变量提升
- `let`是块级作用域，`var`是函数级作用域
- `let`不可在作用域内重复声明同一个变量，`var`可以在同一个作用域内声明同一个变量
- `let`声明的变量不属于顶层对象的属性，`var`声明的变量属于顶层对象的属性
- `let`存在临时性死区，`var`不存在临时性死区

**其他**

1. 在同作用域内，不能同时使用`let`和`var`声明同名变量，不管谁先谁后都不行

### const的实质

`const`实际保证的，并不是变量的值不能改动，而是变量指向的那个`内存地址`所所保存的数据不能改变。

JavaScript中的简单类型数据，比如`string`, `number`, `boolean`, `null`, `undefined`,值就保存在变量指向的那个内存地址，而复合类型的数据，变量指向的那个内存地址，保存的是指向实际数据的一个指针。

### 什么是临时性死区

ES6新概念：临时性死区——TDZ——Temporal Dead Zone

由于代码（代码块，函数，模块......）中的变量还没有被初始化而不能使用的情况，具体表现为——报错：`Uncaught ReferenceError: xxx is not defined`，`let`,`const`,`class`都有临时性死区的表现。在ES6之前，如果在变量初始化之前使用变量，并不会报错，只是其值为`undefined`而已。

### 模板字符串

1. 模板字符串简化了多行字符串的写法
2. 模板字符串简化了在字符串中签入变量的写法
3. 模板字符串中的变量如果没有声明的话，会报错
4. 模板字符串默认会将字符串转义

### ES6中的函数

1. 函数参数可以设置默认值
2. 函数参数的默认值是`惰性求值`的
3. 函数参数设置默认值后build影响函数的`length`属性：`function add(a, b, c=3){} add.length === 2;// true`
4. 在ES6中，如果函数参数使用了默认值、解构赋值、扩展运算符，就不能在函数内部显式指定为严格模式。函数指定默认值后，显式添加`use strict`,报错：`Uncaught SyntaxError: Illegal 'use strict' directive in function with non-simple parameter list`
5. `rest`参数只能是尾参数
6. `rest`参数不计入函数的`length`属性
7. `rest`参数是一个真正的数组，`arguments`是类数组
8. 在ES6中，`name`属性会返回实际的函数名

### 箭头函数

1. 函数体内的`this`就是定义时的对象，而不是使用时的对象
2. 箭头函数不可以当做构造函数
3. 箭头函数内部不存在`arguments`对象，使用`...rest`参数代替
4. 箭头函数不可以做`Generator`函数
5. 箭头函数无法使用`apply`,`bind`,`call`改变`this`指向

### 解构赋值

可解构赋值的：

- 数组
- Set
- 字符串
- 对象
- 函数参数

1. 数组解构赋值：

```
// a === 12 
// b === 33
const [a, b] = [12, 33]; 
```

1. 数组解构默认值：

```
// f === 120 
// h === 56
const [f, h = 100] = [120, 56];
```

1. 数组解构默认值：

```
// f === 120 
// h === 100
const [f, h = 100] = [120, undefined];
```

1. 对象解构赋值：

```
// a === 'Pelli' 
// b === 89
const {a, b} = {a: 'Pelli', b: 89}; 
```

1. 对象解构赋值：

```
// a === 'pelli 
// b === 18 
// c === 'worker'
const {
    myname: a, 
    age: b, 
    job: c
} = {
    myname: 'pelli', 
    age: 18, 
    job: 'worker'
}; 
```

1. 对象解构默认值：

```
// p === 'ppp' 
// q === 'hello world'
const {p, q = 'hello world'} = {p: 'ppp', q: 'qqqq'};
```

1. 解构赋值的默认值需undefined触发，
   - 对于数组来说，对应位置没有元素
   - 对于对象来说，没有同名属性
   - 或者将同名属性或元素显式赋值为undefined
2. 字符串解构赋值：

```
// a === 'h' 
// b === 'e' 
// c === 'l'
const [a, b, c] = 'hello world'; 
```

1. 函数参数的解构赋值

```
const args = function(){return arguments;}
const ags = args(3, 4, 5, 6, 12, 2, 3);
// a === 3 
// b === 4 
// c === 5 
// d === 6 
// e === 12
// f = 2
const [a, b, c, d, e, f] = ags;
```

1. 解构赋值时，如果等号右边是数值和布尔值，则会先转为对象

### 解构赋值圆括号

> 最佳实践：任何时候都不要在解构赋值中放置圆括号

**以下情况不能使用**

1. 变量声明语句
2. 函数参数
3. 赋值语句模式

**只有一种情况可以使用圆括号**

1. 赋值语句的非`模式`部分

### 解构赋值使用场景

1. 变量交换
2. 从函数返回多个值
3. 定义函数参数
4. 提取json数据
5. 定义函数参数默认值
6. 遍历Map结构
7. 输入模块指定方法

### Symbol

1. `Symbol`不是构造函数，定义一个`Symbol`，前不用加`new`，正确使用方式为： `const s = Symbol()`
2. `Symbol`值作为对象属性名时，只能使用`[]`方式访问，不能通过点运算符访问属性
3. `Symbol`是独一无二的值

### Set和Map

1. Set类似于数组，但是其中的值都是唯一的，不可重复 
2. Set构造函数的参数必须是可遍历的：`arguments`,`string`,`array`,`set`,`map`
3. Set构造函数的参数只有第一个有效（只需要一个参数）
4. Set的`add`方法返回的是`Set`对象，可以链式调用
5. Map的`set`方法，返回的是`Map`对象，可以链式调用
6. Set内部只能存在一个`NaN`
7. `Object.is(0, -0); // false`，但是，Set内部`0`和`-0`相等，只能存在一个.
8. Set内部：两个空对象，空数组，空函数总是不相等
9. Map数据结构是键值对的集合
10. Map数据结构的键不仅仅可以是字符串，任何数据类型都可以作为键

### for...of...循环

- arguments
- 数组
- 字符串
- Set
- Map

### Promise的特点

1. Promise对象的状态不受外界影响
2. Promise对象的状态一旦确定就不会再改变
3. Promise对象的状态变化只可能是两个结果中的一个：接受或拒绝

### Promise的缺点

1. Promise对象一旦创建，就无法中途取消
2. 如果不设置回调函数，Promise对象内部抛出的错误不会反映到外部
3. 当Promise对象处于`pending`时，无法得知目前的进展在哪一个阶段，换一种说法是无法确定事件的完成度
4. 代码冗余，相对于回调函数来说，语义弱化。不管什么操作都是一堆then

### Promise对象和其他代码的执行顺序

1. 立即执行的Promise比外层其他代码后执行
2. 立即执行的Promise比`setTimeout 0`先执行

`Promise`本身并不是异步的，`Promise`中的“异步”特性是由`resolve`或者`reject`的执行时机带来的

```
// 下面几行代码输出顺序为：3,2,1
setTimeout(() => {
    console.log('1');
}, 0);

var p = new Promise((resolve, reject) => {
    resolve('2')
});

p.then((val) => {
    console.log(val, 'promise');
})

console.log('3');
```

另外的几行代码：

```
// 下面几行代码输出顺序为：3,1,2
setTimeout(() => {
    console.log('1');
}, 0);

var p = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('2')
    }, 0);
});

p.then((val) => {
    console.log(val, 'promise');
})

console.log('3');
```

### Promise对象的写法最佳实践

参考来源：<http://es6.ruanyifeng.com/#docs/promise>

理由是下面这种写法可以捕获前面then方法执行中的错误（也就是说，catch不仅可以捕获到promise被拒绝的状态，也可以捕获前面then方法中的错误），也更接近同步的写法（try/catch）。因此，建议总是使用catch方法，而不使用then方法的第二个参数。

`p.then(function(val){}).catch(function(msg){})`的写法

```
var p = new Promise(function(resolve, reject){
    setTimeout(function(){
        var status = Math.random() * 10;

        if(status > 5){
            resolve(status);
        }else{
            reject('失败');
        }
    }, 10000);
});

p.then(function(value){
    console.log(value);
}).catch(function(msg){
    console.log(msg);
});
```

不推荐的写法如下：

`then`方法传递两个参数，第二参数是当被拒绝的时候执行的函数

```
var p = new Promise(function(resolve, reject){
    setTimeout(function(){
        var status = Math.random() * 10;

        if(status > 5){
            resolve(status);
        }else{
            reject('失败');
        }
    }, 10000);
});

p.then(function(value){
    console.log(value);
}, function(msg){
    // 这里只有在promise被拒绝的时候才会执行
    // 如果then方法报错了，这里无法获知
    console.log(msg);
});
```

### Promise的其他一些常识

- `Promise`不能直接做函数调用，即不能：`Promise()`

### class的一些常识

- `class`可以看做是语法糖，只是让对象原型的写法更加清晰，更像面向对象的编程语言
- 类的数据类型是函数，类本身指向构造函数
- 类不能做函数直接调用，必须和`new`一起使用
- 类没有变量提升
- 类的所有方法都定义在`prototype`上面
- 类内部定义的所有属性都是不可枚举的
- 类的`length`属性值是`constructor`的参数个数（不包括有默认值的参数）

### ES6中的`NaN`

1. `Number`扩展，`Number.isNaN()`
2. 在`ES6`中，`window`的方法`isNaN()`是为了保证向下兼容性，在`ES6`中，建议使用`Nunber.isNaN()`
3. `Object.is()`中，`NaN`和`NaN`是相等的，`Object.is(NaN, NaN) === true`
4. `Set`中，只允许存在一个`NaN`

### ES6中的模块

- 模块相关名词：`CommonJS`、`AMD`、`CMD`、`UMD`,`ES6 Module`
- ES6之前的模块规范：`CommonJS`和`AMD`,`CommonJS`用于服务器，`AMD`用于浏览器
- ES6的模块设计思想是尽量静态化，使得在`编译时`就能确定模块的依赖关系以及输入输出的变量
- `CommonJS`和`AMD`模块，都只能在`运行时`确定这些东西，比如，`CommonJS`模块就是对象，输入时必须查找对象属性
- ES6模块是`编译时`加载，使得静态分析成为可能
- ES6模块自动采用严格模式，不管有没有显式声明`"use strict;"`
- 在ES6模块中，顶层的`this`指向`undefined`
- 如果A模块引入的模块B是基本类型，A对B重新赋值的话会报错，如果B是引用类型，A对其属性重新赋值， B的属性值会改变，如果别的模块（比如C模块）也引用了B模块，则A对B的属性值修改也会体现在C模块中
- 最佳实践：不要对引入的模块进行修改
- 模块中的`import`有提升效果，会提升到整个模块之前。提升效果的实质是：`import`命令是`编译阶段`执行的，编译阶段总是在代码实际运行之前
- 由于`import`是静态执行的，不能使用表达式和变量，因为表达式和变量只有在运行时才能获取到结果
- 如果多次执行同一条`import`语句，则只会真正执行一次，不会执行多次
- ES6模块中的几个关键语句：`import 'react';`,`import {lodash} from 'lodash';`, , `import * from 'react'`, `export var a = 12;`, `const a = 55; export default a;`
- 正是因为`export default`实际上是输出一个名为`default`的变量，所以不能在`export default`后面加变量声明语句
- ES6模块无法动态加载、按条件加载、运行时加载。因为`import`语句是在编译时执行的，是静态的

### CommonJS和ES6模块的不同

- CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。
- CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。
- Node环境中，ES6 模块之中，顶层的`this`指向`undefined`；`CommonJS` 模块的顶层`this`指向当前模块，这是两者的一个重大差异。
- 在Node环境中，以下几个变量在模块中是不存在的：`arguments`,`require`,`module`,`exports`,`__filename`,`__dirname`

### ES6模块的运行机制

ES6 模块的运行机制与 CommonJS 不一样。JS 引擎对脚本静态分析的时候，遇到模块加载命令import，就会生成一个只读引用。等到脚本真正执行时，再根据这个只读引用，到被加载的那个模块里面去取值。换句话说，ES6 的import有点像 Unix 系统的“符号连接”，原始值变了，import加载的值也会跟着变。因此，ES6 模块是动态引用，并且不会缓存值，模块里面的变量绑定其所在的模块。