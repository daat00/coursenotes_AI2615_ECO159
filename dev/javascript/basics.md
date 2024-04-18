Javascript Basics
===
javascript 语言不像 java，更像 C crossed with Self/Scheme
- 语法类似 C，一切皆对象，垃圾回收

js = ECMAScript + DOM + BOM，基本语法遵循 ECMAScript 语法规范
- high-level, dynamic, untyped, interpreted
- prototype-based, first-class functions
- supports object-oriented, imperative, and functional programming styles
- API for working with text, arrays, dates, regular expressions, and the DOM


js 是解释型脚本语言，语言在设计上存在一些问题，可以用 `use strict` 限制功能

## 定义变量
### var, let, const
js 的变量分为 global 和 function local
- `var` 声明一个 undefined 变量，声明的变量会被`提升` (hoisted) 到函数的顶部
- ES6 新增 `let` 和 `const`，`let`，可以声明 block-scoped 变量


### Primitive Types
```js
undefined, number, string, boolean, object, function
```

`number` 是 double，NaN, Infinity 是特殊 number 变量。位运算操作符都是 32bit 的

`string` 是动态的，类似 python immutable

null == undefined; null !== undefined

### object
即 dict，可以通过 bar.name 或 bar['name'] 访问属性
- global scope 是浏览器中的一个对象（i.e. window[prop]）

### array
array 是 object 的特化类型，`typeof arr` 是 object。
- 通过索引访问元素
- 支持默认 undefined 直接插入，或跨越插入，中间的 undefined

### function
function 是一等公民，并且声明是 hoisted 的

function 也是 object，可以定义属性和方法
```js
function foo() {
    foo.bar = 1;
}
foo.toString(); // "function foo() { foo.bar = 1; }"
foo.bind(this_, args);
```

ES6 新增箭头函数 `=>`，不 redefine `this`

### RegExp
```js
let re = /ab+c/; // new RegExp('ab+c')
```

### this
若函数是 object 的成员，如 
```js
o.f = function(){this};
```
js 指向 o

在其他时候，若 `use strict`，this 是 undefined；否则是 global object

### class
class 也用 function 定义
```js
function Rectangle(height, width) {
    this.height = height;
    this.width = width;
    this.area = function() {
        return this.height * this.width;
    }// 一般不这么写
}

Rectangle.prototype.area = function() {
    return this.height * this.width;
}
```

#### class: ES6 extension
```js
class Rectangle extends Shape {
    constructor(height, width) {
        super(height, width);
    }
    /* static */ area() {
        return this.height * this.width;
    }
}
```

#### Prototype
prototype 的继承关系构成  prototype chain，直到 null
- 读取属性时，会沿着 prototype chain 查找
- 修改属性时，总会在当前 obj 找到/新建属性

动态修改 prototype 会影响所有实例

### closure
```js
function makeAdder(x) {
    return function(y) {
        return x + y;
    };
}
```
closure 保存了函数定义时的环境，可以访问外部变量
- 类比 lambda 函数，在闭包类把捕获变量保存了

#### 用闭包实现 private 变量
```js
let myObj = (function() { 
   let privateProp1 = 1;
   let compute = function() {return privateProp1 + privateProp2;}
   return {compute: compute};
})();
```


闭包 bug
```js
for (var fileNo = 0; fileNo < 2; fileNo++) {
fs.readFile('./file' + fileNo, function (err, data) {
       if (!err) { 
   console.log('file', fileNo, 'has length', data.length);
       }
    });
}
```
打印的 fileNo 都是 2，因为闭包捕获了变量，且是异步的

用 let 可以正确捕获变量，因为 fileNo 的 scope 是整个循环，而 {} 内定义的变量是 block scope

# New Features
### Transpilers
translate new features to old js
- Babel: ES6 -> ES5

前端框架 aggressively 使用 new features

### Modules
export/import

### Rest Parameters
```js
function f(a, b, ...args) {
    // instead of arguments[2]
}
```
### Spread Operator
...args

### Destructuring
```js
let [a, b] = [1, 2];
let {a, b} = {a: 1, b: 2};
```

### strings
```js
let s = `hello ${name}`;
let s = `multi
line`;
```

### for-of

### Set, Map, WeakSet, WeakMap

### async/await, Promise
```js
async function f() {
    let result = await Promise.resolve(1);
    console.log(result);
}
```

解决异步函数（非阻塞）的回调地狱（嵌套）

async 声明函数永远返回一个 promise

### EcmaScript 2020
`?.` optional chaining

BigInt

vue, alpine
===
1. ~~this~~, $refs, $data
2. 本质上还是由 js 解释 html，所以，在引擎 start 之前的时候，变量不存在；同时，this 指向全局，tag 中定义的变量不容易拿到，$data