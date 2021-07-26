# promise对象（未发布）

`Promise` 是异步编程的一种解决方案，比传统的解决方案——**回调函数和事件**——更合理和更强大。它由社区最早提出和实现，`ES6`将其写进了语言标准，统一了用法，原生提供了`Promise`对象。

## 什么是promise对象

所谓`Promise`，简单说就是一个*容器*，里面保存着某个未来才会结束的事件（通常是一个**异步**操作）的结果。

从语法上说，`Promise` 是一个对象，从它可以获取异步操作的消息。`Promise` 提供统一的` API`，各种异步操作都可以用同样的方法进行处理。

对于`Promise`对象有两个特点：

- 对象的操作不受外界的影响（`Promise`对象有三个状态分别为：*pending*(**进行中**)，*fulfilled*(**已成功**)，*rejected*(**已失败**)）
- 状态如果发生改变，就不会再次发生改变（`Promise`对象的状态改变只有两种可能，当发生改变时，状态就会凝固，此时的状态就是*resolved*(**已定型**)）

## Promise对象的使用

`ES6`规定,`Promise`对象是一个**构造函数**，用来生成`promise`实例。

### 参数

`Promise`构造函数接受一个函数作为参数，该函数的两个参数分别是`resolve`和`reject`。它们是两个函数，由 `JavaScript` 引擎提供，不用自己部署。

####  resolve

`resolve`函数的作用是，将`Promise`对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去;

#### reject

`reject`函数的作用是，将`Promise`对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

下面是`Promise`的一个简单例子，

~~~javascript
function timeout(ms) {
  return new Promise((resolve, reject) => {
    setTimeout(resolve, ms, 'done');
  });
}

timeout(100).then((value) => {
  console.log(value);
});
~~~

![img](https://img-blog.csdnimg.cn/20200210203753463.png)

在上面的例子中，`timeout`方法返回一个`Promise`实例，表示一段时间以后才会发生的结果。我们可以看到，在100`ms`时间结束后会先讲状态变成`resolved`,然后调用回调函数打印参数`value`，也就是`done`这个字符串。

>`Promise`新建的函数会立即执行



~~~javascript
let promise = new Promise(function(resolve, reject) {
  console.log('Promise');
  resolve();
});

promise.then(function() {
  console.log('resolved.');
});

console.log('Hi!');

// Promise
// Hi!
// resolved
~~~

这个函数就会先打印`'Promise`',其次就是非异步执行的打印出来`'Hi!'`

## `Promise`对象的方法

- `Promise.prototype.then()`
- `Promise.prototype.catch()`
- `Promise.prototype.finally()`
- `Promise.all()`
- `Promise.race()`
- `Promise.any()`
- `Promise.resolve()`
- `Promise.reject()`

------



### `Promise.prototype.then()`
首先我们要知道，`Promise`实例具有`then`方法，就意味着`then`方法定义于`Promise`的原型对象上。它的作用是为`Promise`添加状态改变的回调函数，前面我们提到，`then`方法的第一个参数是`resolved`状态的回调函数，第二个参数（*可选*）是`rejected`状态的回调函数。

> `then`方法返回的是一个**新的**`Promise`实例，所以我们可以采用链式写法，依次指定多个回调函数。

```javascript
getJSON("/posts.json").then(function(json) {
  return json.post;
}).then(function(post) {
  // ...
});
```

上述代码中第二个`then`的参数值是第一个`then`的返回结果，从而指定了两个回调函数。

### `Promise.prototype.catch（）`

`Promise.prototype.catch`方法是`.then(null, rejection)`或`.then(undefined, rejection)`的别名，用于指定发生错误时的回调函数。

```javascript
getJSON('/posts.json').then(function(posts) {
  // ...
}).catch(function(error) {
  // 处理 getJSON 和 前一个回调函数运行时发生的错误
  console.log('发生错误！', error);
});
```

上面代码中，`getJSON`方法返回一个 `Promise`对象，如果该对象状态变为`resolved`，则会调用`then`方法指定的回调函数；如果异步操作抛出错误，状态就会变为`rejected`，就会调用`catch`方法指定的回调函数，处理这个错误。另外，`then`方法指定的回调函数，如果运行中抛出错误，也会被`catch`方法捕获。



> 注意：跟传统的`try/catch`代码块不同的是，如果没有使用`catch`方法指定错误处理的回调函数，`Promise` 对象抛出的错误不会传递到外层代码，即不会有任何反应。

~~~javascript
const someAsyncThing = function() {
  return new Promise(function(resolve, reject) {
    // 下面一行会报错，因为x没有声明
    resolve(x + 2);
  });
};

someAsyncThing().then(function() {
  console.log('everything is great');
});

setTimeout(() => { console.log(123) }, 2000);
// Uncaught (in promise) ReferenceError: x is not defined
// 123
~~~

上面代码中，`someAsyncThing`函数产生的 `Promise` 对象，内部有语法错误。浏览器运行到这一行，会打印出错误提示`ReferenceError: x is not defined`，但是不会退出进程、终止脚本执行，2 秒之后还是会输出`123`。这就是说，`Promise` 内部的错误不会影响到` Promise` 外部的代码，通俗的说法就是“`Promise` 会吃掉错误”。