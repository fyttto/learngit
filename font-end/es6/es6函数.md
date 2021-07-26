---
title: ES6函数
date: 2019-12-02 22:26:33
tags:- ES6
-Web前端
categories: -Web前端
---

在这篇博文里，我们大家会一起了解到许许多多在`ES6`中的函数的新特性。

比如函数的尾调用，箭头函数，函数参数的默认值以及返回值的变化。

<!--more-->

# 函数的拓展

##  参数的默认值

对于参数的默认值来说，在`ES6` 中我们现在可以为函数指定一个*默认值*，在定义参数时候写它的默认值即可。

~~~javascript
function log(x,y='World'){
    console.log(x,y);
}
log('hello');	//hello World
log('hello','China');	//hello China
log('hello','');	//hello World
~~~

> 在`ES6` 以前，我们可以通过变通的方式添加函数参数的默认值。但是我们并不能保证当我们传入参数时参数不使用默认值。

~~~js
function log(x, y) {
  y = y || 'World';
  console.log(x, y);
}
log('Hello', '') // Hello World
~~~

上面的代码中，我们可以看到，最后一行传入的参数值为一个**空字符**，但是其对应的布尔值为`false` ,所以该赋值将不起作用。

> 这里有个注意事项就是，我们不能在使用默认参数时调用同名参数，也不能在使用默认参数值时候再次声明变量否则同样会报错。ヾ|≧_≦|〃

~~~javascript
//不报错
function foo(x,x,y){
    //...
}
//报错
function foo(x,x,y=1){
    //...
}
// SyntaxError: Duplicate parameter name not allowed in this context

__________________________________________________


function foo(a=1){
    let a=1;
    const a=2;
}
//error
~~~

在简洁方面，`ES6`的写法会使阅读代码的人**更容易看懂哪些参数是可以省略**；其次，也**优化了代码**；

我们之前了解过*变量的解构赋值*，在这里我们也可以将它们结合起来使用。

~~~javascript
function foo({x, y = 5}) {
  console.log(x, y);
}

foo({}) // undefined 5
foo({x: 1}) // 1 5
foo({x: 1, y: 2}) // 1 2
foo() // TypeError: Cannot read property 'x' of undefined
__________________________________________________


function foo({x, y = 5} = {}) {
  console.log(x, y);
}

foo() // undefined 5
~~~

比如上面的第一个函数的参数是一个对象，下面参数为一个*空对象*时，它的属性的值就是我们为它预设的*默认值*，但是如果我们没有传入参数时，就会报错；但是我们如果将参数的默认值设置为一个**空对象**时，不传参数的方式调用函数就<u>不会报错</u>。

在指定了默认值之后，函数就会拥有一个属性，称作函数的`length`属性，会**返回没有指定默认值的参数个数**。所以指定这个属性时，需要我们将函数的参数写在特定的位置——尾参数。

~~~javascript
(function (a) {}).length // 1
(function (a = 5) {}).length // 0
(function (a, b, c = 5) {}).length // 2
__________________________________________________

(function (a = 0, b, c) {}).length // 0
(function (a, b = 1, c) {}).length // 1
//这个属性已经失真
~~~



> 其实在使用默认值的时候，为了避免一些错误，我们最好将带有默认值的一些参数写在尾参数部分。

~~~javascript
function f(x, y = 5, z) {
  return [x, y, z];
}
f() // [undefined, 5, undefined]
f(1) // [1, 5, undefined]
f(1, ,2) // 报错
f(1, undefined, 2) // [1, 5, 2]
__________________________________________________

function foo(x = 5, y = 6) {
  console.log(x, y);
}

foo(undefined, null)
// 5 null
~~~

这里没有指定默认值的参数，我们必须显式的传入参数`undefined` ，否则将会报错，这就带来许多不便。

但是如果我们将参数写在尾参数位置上，能为我们减少很多麻烦。

~~~javascript
function loo(x,y,z=5){
    console.log(x,y,z);
}
loo(1,2);
~~~







## rest参数

`ES6`引入`rest`参数（形式为`...变量名`），可以用来获取函数的多余参数，这样的话就无需使用`argument`对象了。`rest`本质是一个数组，所以我们可以使用数组的方法。对比`argument`对象就没有这样便捷的方式，我们要将其转换为数组才可以使用数组的方法。

~~~javascript
function add(...values) {
  let sum = 0;
  for (var val of values) {
    sum += val;
  }
  return sum;
}
add(2, 5, 3) // 10
~~~

这段代码中的是一个`add`求和函数，利用了rest参数，可以向该函数传入任意数目的参数，都可以进行求和运算。

~~~javascript
//rest参数使用数组的方法
function push(array, ...items) {
  items.forEach(function(item) {
    array.push(item);
    console.log(item);
  });
}

var a = [];
push(a, 1, 2, 3)
~~~

上面的代码改写了新的`push`方法，并在push过程中打印了数组。

~~~javascript
// arguments变量的写法
function sortNumbers() {
  return Array.prototype.slice.call(arguments).sort();
}

// rest参数的写法
const sortNumbers = (...numbers) => numbers.sort();
~~~

`argument`是一个类似于数组的对象，上面使用`argument`变量时，必须将`argument`对象变成数组(`Array.prototype.slice.call`)才可以使用数组的方法。

## `name`属性

函数的`name`属性，会返回该函数的函数名。

~~~javascript
function foo(){}
foo.name
//“foo”
~~~

上面的代码就是个简单的例子，

1. 对于`ES5`，对于匿名函数赋值给一个变量，会返回一个空字符串，而`ES6`的`name`属性会返回实际的函数名。

~~~javascript
var f=function()

//ES5
f.name	//""

//ES6
f.name 	//"f"
~~~

2. 对于`ES5`，对于具名函数赋值给一个变量会返回这个具名函数原本的名字，同样`ES6`也是。

~~~javascript
const bar=function baz(){};
//ES5
bar.name;	//"baz"
//ES6
bar.name;	//"baz"
~~~

3. `Function` 构造函数返回的函数实例，`name`属性的值为`anonymous`。

而`bind`返回的函数，其`name`属性值会加上`bound`前缀。

~~~javascript
(new Function).name	//"anonymous"

function foo(){};
foo.bind({}).name	//"bound foo"
(function(){}).bind({}).name	//"bound"
~~~

上面的函数分别时一个`foo`名称的函数和一个匿名函数，所以前面加上`bound`前缀的结果就是上面的结果。

## 箭头函数

### 定义

`ES6`允许使用箭头（`=>`）来定义一个函数。

~~~javascript
var f=v=>v;

//等于
var f=function(v){
    return v;
};
~~~

### 参数

在箭头函数中，如果需要多个参数或者无需参数，我们只需要使用一个圆括号来代表参数部分。

~~~javascript
var f=()=>5;
//等同于
var f=function(){ return 5};
___________________
var sum=(num1,num2)=>num1+num2;
//等同于
var sum=function(num1,num2){
    return num1+num2;
};
~~~

### 返回值以及代码块问题

如果在箭头函数中**代码块部分多于一条语句**，就要使用大括号将它们括起来，并使用`return`语句返回。

第二种情况是当箭头函数需要**返回一个对象**的时候，就必须在返回值前面加上大括号，否则就会报错。



~~~javascript
var sum=(num1,num2)=>{return num1+num2;}

//返回一个对象的时候
let getTempItem = id => { id: id, name: "Temp" };
//报错
let getTempItem = id => ({ id: id, name: "Temp" });
//不报错
~~~

当没有返回值的时候，我们可以采用调用一个函数的写法：

~~~javascript
let fn=()=>void doesNotReturn();
~~~

### 箭头函数的优点

1. 箭头函数使得表达更加简洁

   ~~~javascript
   const isEven = n => n % 2 === 0;
   const square = n => n * n;
   ~~~

   上面定义的两个函数仅仅使用了少量的代码定义了两个工具函数。

2. 箭头函数可以与变量的解构结合使用

   ~~~java
   const full = ({ first, last }) => first + ' ' + last;
   
   // 等同于
   function full(person) {
     return person.first + ' ' + person.last;
   }
   ~~~

   箭头函数可以有效的减少代码的使用量。

3. 简化回调函数

   ~~~javascript
   // 正常函数写法
   [1,2,3].map(function (x) {
     return x * x;
   });
   
   // 箭头函数写法
   [1,2,3].map(x => x * x);
   
   ~~~

### 注意点

>箭头函数有几个使用注意点。
>
>（1）函数体内的`this`对象，就是定义时所在的对象，而不是使用时所在的对象。
>
>（2）不可以当作构造函数，也就是说，不可以使用`new`命令，否则会抛出一个错误。
>
>（3）不可以使用`arguments`对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。
>
>（4）不可以使用`yield`命令，因此箭头函数不能用作 Generator 函数。

`this`对象在定义函数时，已经决定，并不是在使用时根据使用对象改变。

**我们都知道`this` 的指向是可变的 ，但在箭头函数中，`this`指向不可变** 

~~~javascript
function foo() {
  setTimeout(() => {
    console.log('id:', this.id);
  }, 100);
}

var id = 21;

foo.call({ id: 42 });
// id: 42
~~~

根据我们之前的理解，`this`指向的对象是定义时所在的定义域，而不是运行所在的定义域。

**箭头函数可以让`this`指向固定化，这种特性很有利于封装回调函数。下面是一个例子，DOM 事件的回调函数封装在一个对象里面。**

~~~javascript
var handler = {
  id: '123456',

  init: function() {
    document.addEventListener('click',
      event => this.doSomething(event.type), false);
  },

  doSomething: function(type) {
    console.log('Handling ' + type  + ' for ' + this.id);
  }
};
~~~

上面代码的`init`方法中，使用了箭头函数，这导致这个箭头函数里面的`this`，总是指向`handler`对象。否则，回调函数运行时，`this.doSomething`这一行会报错，因为此时`this`指向`document`对象。

`this`指向的**固定化**，并不是因为箭头函数内部有绑定`this`的机制，*实际原因**是箭头函数根本没有自己的`this`，导致内部的`this`就是外层代码块的`this`。正是因为它没有`this`*，所以也就**不能用作构造函数**。

**除了`this`，以下三个变量在箭头函数之中也是不存在的，指向外层函数的对应变量：`arguments`、`super`、`new.target`。**

```javascript
function foo() {
  setTimeout(() => {
    console.log('args:', arguments);
  }, 100);
}

foo(2, 4, 6, 8)
// args: [2, 4, 6, 8]
```

**另外，由于箭头函数没有自己的`this`，所以当然也就不能用`call()`、`apply()`、`bind()`这些方法去改变`this`的指向。**

```javascript
(function() {
  return [
    (() => this.x).bind({ x: 'inner' })()
  ];
}).call({ x: 'outer' });
// ['outer']
```

上面的代码中，箭头函数没有自己的`this`，所以`bind`方法是无效的。内部的`this`方法也指向`this`。

### 不适用范围

箭头函数使得`this`从“动态”变成“静态”。基于这个原因就有两个场景不适合

#### 定义对象的方法

当有一个方法内部包括`this`，不可以使用箭头函数。

~~~javascript
const cat = {
  lives: 9,
  jumps: () => {
    this.lives--;
  }
}
~~~

因为在上述代码中，`cat.jump()`是哟个箭头函数，这是个错误的写法。在调用时，普通函数会使得`this`指向对象，而箭头函数会使得`this`指向全局对象，并不会得到预期的结果。（原因：对象并不会构成单独的作用域）

#### 定义动态`this `的时候

```javascript
var button = document.getElementById('press');
button.addEventListener('click', () => {
  this.classList.toggle('on');
});
```

在为一个DOM元素定义监听函数的时候，不可使用箭头函数，因为如果将其设置为箭头函数，那么

它的`this`就是全局的对象，点击按钮就会报错。

> 如果函数体很复杂，有许多行，或者函数内部有大量的读写操作，不单纯是为了计算值，这时也不应该使用箭头函数，而是要使用普通函数，这样可以提高代码可读性。

### 箭头函数的嵌套使用

下面是一个部署管道机制（pipeline）的例子，即前一个函数的输出是后一个函数的输入。

```javascript
const pipeline = (...funcs) =>
  val => funcs.reduce((a, b) => b(a), val);

const plus1 = a => a + 1;
const mult2 = a => a * 2;
const addThenMult = pipeline(plus1, mult2);

addThenMult(5)
// 12
```

如果觉得上面的写法可读性比较差，也可以采用下面的写法。

```javascript
const plus1 = a => a + 1;
const mult2 = a => a * 2;

mult2(plus1(5))
// 12
```

## 尾调用

尾调用（Tail Call）是函数式编程的一个重要概念，本身非常简单，一句话就能说清楚，就是指某个函数的最后一步是调用另一个函数。

```javascript
function f(x){
  return g(x);
}
```

上面代码中，函数`f`的最后一步是调用函数`g`，这就叫尾调用。

以下三种情况，都不属于尾调用。

```javascript
// 情况一
function f(x){
  let y = g(x);
  return y;
}

// 情况二
function f(x){
  return g(x) + 1;
}

// 情况三
function f(x){
  g(x);
}
```

上面代码中，情况一是调用函数`g`之后，还有赋值操作，所以不属于尾调用，即使语义完全一样。情况二也属于调用后还有操作，即使写在一行内。情况三等同于下面的代码。

```javascript
function f(x){
  g(x);
  return undefined;
}
```

尾调用不一定出现在函数尾部，只要是最后一步操作即可。

```javascript
function f(x) {
  if (x > 0) {
    return m(x)
  }
  return n(x);
}
```

上面代码中，函数`m`和`n`都属于尾调用，因为它们都是函数`f`的最后一步操作。

#### 尾调用优化

即**只保留内层函数的调用帧**。如果所有函数都是尾调用，那么完全可以做到每次执行时，调用帧只有一项，这将大大节省内存。这就是“尾调用优化”的意义。

```javascript
function f() {
  let m = 1;
  let n = 2;
  return g(m + n);
}
f();

// 等同于
function f() {
  return g(3);
}
f();

// 等同于
g(3);
```

上面代码中，如果函数`g`不是尾调用，函数`f`就需要保存内部变量`m`和`n`的值、`g`的调用位置等信息。但由于调用`g`之后，函数`f`就结束了，所以执行到最后一步，完全可以删除`f(x)`的调用帧，只保留`g(3)`的调用帧。



> 尾调用之所以与其他调用不同，就在于它的*特殊的调用位置*。



我们知道，函数调用会在内存形成一个“调用记录”，又称“*调用帧*”（call frame），**保存调用位置**和**内部变量**等信息。如果在函数`A`的内部调用函数`B`，那么在`A`的调用帧上方，还会形成一个`B`的调用帧。等到`B`运行结束，将结果返回到`A`，`B`的调用帧才会消失。如果函数`B`内部还调用函数`C`，那就还有一个`C`的调用帧，以此类推。所有的调用帧，就形成一个“调用栈”（call stack）。



尾调用由于是函数的最后一步操作，所以不需要保留外层函数的调用帧，因为调用位置、内部变量等信息都不会再用到了，只要直接用内层函数的调用帧，取代外层函数的调用帧就可以了。



> 注意，只有不再用到外层函数的内部变量，内层函数的调用帧才会取代外层函数的调用帧，否则就无法进行“尾调用优化”。

## `Function.prototype.toString()`

[ES2019](https://github.com/tc39/Function-prototype-toString-revision) 对函数实例的`toString()`方法做出了修改。

`toString()`方法返回函数代码本身，以前会省略注释和空格。

```javascript
function /* foo comment */ foo () {}

foo.toString()
// function foo() {}
```

上面代码中，函数`foo`的原始代码包含注释，函数名`foo`和圆括号之间有空格，但是`toString()`方法都把它们省略了。

修改后的`toString()`方法，明确要求返回一模一样的原始代码。

```javascript
function /* foo comment */ foo () {}

foo.toString()
// "function /* foo comment */ foo () {}"
```



这篇博文仅仅是在借鉴阮一峰老师的博客基础上发表了些自己小小的见解，详细可以查看

[阮一峰老师的博文]: https://es6.ruanyifeng.com/#docs/function	"阮一峰的ES6入门"

