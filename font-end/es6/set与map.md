# set与map（未提交）

在`ES6`中提供了两种新的数据结构分别是`set`和`map`，今天我们就来谈谈这两种数据结构。

## set

首先是`set`，在我看来这种数据结构，仿佛就是为了我们对**某些数据去重**或者是**方便存储某些集合类数据**而设置的。

~~~javascript
const s=new Set();
//先声明一下这个数据结构
~~~



Set具有诸多属性，比如：

- `Set.prototype.constructor`:构造函数，默认为`Set`函数
- `Set.prototype.size`:返回`Set`实例的成员总数



那么它的特性也就显而易见了，这种数据结构类似于数组，存储的数据都是唯一的，不可重复的。

下面我们来说说它的基本用法：

1. 首先，这是一个数据结构，那么我们就要学会向其中**添加**，**删除**数据，**初始化**等操作。

`Set.prototype.add(value)`:添加某个值，返回`set`结构本身；

`Set.prototype.delete(value)`:删除某个值，返回*布尔值*，表示删除是否成功；

`Set.prototype.has(value)`:返回某个值，表示该值是否为`Set`的成员；

`Set.prototype.clear()`:清空`set`中的所有成员，无返回值。

~~~javascript
s.add(1).add(2).add(2);
s.size;
//2
//数字2被添加了两次
s.delete(1);
s.size;
//1

s.has(1);//false
s.has(2);//true

s.clear();//清空所有成员
s.size;
//0
~~~

- `Set.add(value)`是向Set数据结构中添加数据，其中参数可以是基本类型数据，数组，类数组对象等。

~~~javascript
s.add({});
s.size;
// 1

s.add({});
s.size;
// 2
~~~

> 在添加数据时，`Set`数据结构会自动去重，但是多个`NAN`在`Set`内部被认为是一个数据，而且两个空对象总是被认为是不同的对象。



2. 接下来就该遍历在`Set`数据结构中的数据了

   `Set.prototype.keys()`：返回**键名**的遍历器

   `Set.prototype.values()`：返回**值名**的遍历器

   `Set.prototype.entries()`：返回**键值对**的遍历器

   >注意，在遍历过程中，遍历顺序就是添加顺序。在`Set`中没有键名，只有键值，所以说`keys()`和`values`的行为是相同的。而`entries()`返回的是键值对，则返回的就是两个相同的值。

   - `keys()`,`values()`,`entries()`

   ~~~javascript
   s.add("red","green","blue");
   for (let item of s.keys()) {
     console.log(item);
   }
   //red
   //green
   //blue
   
   for (let item of s.values()) {
     console.log(item);
   }
   //red
   //green
   //blue
   
   for (let item of s.entries()) {
     console.log(item);
   }
   
   //["red","red"]
   //["green","green"]
   //["blue","blue"]
   ~~~

   

   `Set`的结构默认可遍历，

   ~~~javascript
   Set.prototype[symbol.iterator] === Set.prototype.values()
   //ture
   ~~~

   所以，我们可以忽略上面的方法，直接使用`for...of`遍历`Set`。

   ~~~JavaScript
   for(let i of s){
       console.log(i);
   }
   //red
   //green
   //blue
   ~~~

   - `forEach()`

     ~~~javascript
     let set = new Set([1, 4, 9]);
     set.forEach((value, key) => console.log(key + ' : ' + value))
     // 1 : 1
     // 4 : 4
     // 9 : 9
     ~~~

> 注意，Set 结构的键名就是键值（两者是同一个值），因此第一个参数与第二个参数的值永远都是一样的。另外，`forEach`方法还可以有第二个参数，表示绑定处理函数内部的`this`对象。

### Set的应用

1. 数组去重

```javascript
// 去除数组的重复成员
[...new Set(array)]
```

上面的方法也可以用于，去除字符串里面的重复字符。

```javascript
[...new Set('ababbc')].join('')
// "abc"
```

`Array.from`方法可以将 Set 结构转为数组。

```javascript
const items = new Set([1, 2, 3, 4, 5]);
const array = Array.from(items);
```

这就提供了去除数组重复成员的另一种方法。

```javascript
function dedupe(array) {
  return Array.from(new Set(array));
}

dedupe([1, 1, 2, 3]) // [1, 2, 3]
```

2. 实现交集，并集和差集

   ```javascript
   let a = new Set([1, 2, 3]);
   let b = new Set([4, 3, 2]);
   
   // 并集
   let union = new Set([...a, ...b]);
   // Set {1, 2, 3, 4}
   
   // 交集
   let intersect = new Set([...a].filter(x => b.has(x)));
   // set {2, 3}
   
   // 差集
   let difference = new Set([...a].filter(x => !b.has(x)));
   // Set {1}
   ```

3. 在遍历操作中，同步改变原来的`Set`结构

   ```javascript
   // 方法一
   let set = new Set([1, 2, 3]);
   set = new Set([...set].map(val => val * 2));
   // set的值是2, 4, 6
   
   // 方法二
   let set = new Set([1, 2, 3]);
   set = new Set(Array.from(set, val => val * 2));
   // set的值是2, 4, 6
   ```

## map



它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。也就是说，`Object` 结构提供了“字符串—值”的对应，`Map` 结构提供了“值—值”的对应，是一种更完善的 `Hash` 结构实现。如果你需要“键值对”的数据结构，`Map` 比 `Object` 更合适。

下面我们来看看`map`的基本属性吧，

**（1）`size` 属性**

`size`属性返回 Map 结构的成员总数。

```javascript
const map = new Map();
map.set('foo', true);
map.set('bar', false);

map.size // 2
```

**（2）`Map.prototype.set(key, value)`**

`set`方法设置键名`key`对应的键值为`value`，然后返回整个 Map 结构。如果`key`已经有值，则键值会被更新，否则就新生成该键。

```javascript
const m = new Map();

m.set('edition', 6)        // 键是字符串
m.set(262, 'standard')     // 键是数值
m.set(undefined, 'nah')    // 键是 undefined
```

`set`方法返回的是当前的`Map`对象，因此可以采用链式写法。

```javascript
let map = new Map()
  .set(1, 'a')
  .set(2, 'b')
  .set(3, 'c');
```

**（3）`Map.prototype.get(key)`**

`get`方法读取`key`对应的键值，如果找不到`key`，返回`undefined`。

```javascript
const m = new Map();

const hello = function() {console.log('hello');};
m.set(hello, 'Hello ES6!') // 键是函数

m.get(hello)  // Hello ES6!
```

**（4）`Map.prototype.has(key)`**

`has`方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。

```javascript
const m = new Map();

m.set('edition', 6);
m.set(262, 'standard');
m.set(undefined, 'nah');

m.has('edition')     // true
m.has('years')       // false
m.has(262)           // true
m.has(undefined)     // true
```

**（5）`Map.prototype.delete(key)`**

`delete`方法删除某个键，返回`true`。如果删除失败，返回`false`。

```javascript
const m = new Map();
m.set(undefined, 'nah');
m.has(undefined)     // true

m.delete(undefined)
m.has(undefined)       // false
```

**（6）`Map.prototype.clear()`**

`clear`方法清除所有成员，没有返回值。

```javascript
let map = new Map();
map.set('foo', true);
map.set('bar', false);

map.size // 2
map.clear()
map.size // 0
```

### 遍历方法

Map 结构原生提供三个遍历器生成函数和一个遍历方法。

- `Map.prototype.keys()`：返回键名的遍历器。
- `Map.prototype.values()`：返回键值的遍历器。
- `Map.prototype.entries()`：返回所有成员的遍历器。
- `Map.prototype.forEach()`：遍历 Map 的所有成员。

> 需要特别注意的是，Map 的遍历顺序就是插入顺序。

```javascript
const map = new Map([
  ['F', 'no'],
  ['T',  'yes'],
]);

for (let key of map.keys()) {
  console.log(key);
}
// "F"
// "T"

for (let value of map.values()) {
  console.log(value);
}
// "no"
// "yes"

for (let item of map.entries()) {
  console.log(item[0], item[1]);
}
// "F" "no"
// "T" "yes"

// 或者
for (let [key, value] of map.entries()) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"

// 等同于使用map.entries()
for (let [key, value] of map) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"
```

### 与其他数据结构的互相转换

**（1）`Map` 转为数组**

前面已经提过，Map 转为数组最方便的方法，就是使用扩展运算符（`...`）。

```javascript
const myMap = new Map()
  .set(true, 7)
  .set({foo: 3}, ['abc']);
[...myMap]
// [ [ true, 7 ], [ { foo: 3 }, [ 'abc' ] ] ]
```

**（2）数组 转为 `Map`**

将数组传入 Map 构造函数，就可以转为 Map。

```javascript
new Map([
  [true, 7],
  [{foo: 3}, ['abc']]
])
// Map {
//   true => 7,
//   Object {foo: 3} => ['abc']
// }
```

**（3）`Map` 转为对象**

如果所有 `Map` 的键都是字符串，它可以无损地转为对象。

```javascript
function strMapToObj(strMap) {
  let obj = Object.create(null);
  for (let [k,v] of strMap) {
    obj[k] = v;
  }
  return obj;
}

const myMap = new Map()
  .set('yes', true)
  .set('no', false);
strMapToObj(myMap)
// { yes: true, no: false }
```

如果有非字符串的键名，那么这个键名会被转成字符串，再作为对象的键名。

**（4）对象转为 `Map`**

```javascript
function objToStrMap(obj) {
  let strMap = new Map();
  for (let k of Object.keys(obj)) {
    strMap.set(k, obj[k]);
  }
  return strMap;
}

objToStrMap({yes: true, no: false})
// Map {"yes" => true, "no" => false}
```

**（5）`Map` 转为 `JSON`**

Map 转为 JSON 要区分两种情况。一种情况是，Map 的键名都是字符串，这时可以选择转为对象 JSON。

```javascript
function strMapToJson(strMap) {
  return JSON.stringify(strMapToObj(strMap));
}

let myMap = new Map().set('yes', true).set('no', false);
strMapToJson(myMap)
// '{"yes":true,"no":false}'
```

另一种情况是，`Map` 的键名有非字符串，这时可以选择转为数组 `JSON`。

```javascript
function mapToArrayJson(map) {
  return JSON.stringify([...map]);
}

let myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);
mapToArrayJson(myMap)
// '[[true,7],[{"foo":3},["abc"]]]'
```

**（6）`JSON` 转为 `Map`**

`JSON `转为 Map，正常情况下，所有键名都是字符串。

```javascript
function jsonToStrMap(jsonStr) {
  return objToStrMap(JSON.parse(jsonStr));
}

jsonToStrMap('{"yes": true, "no": false}')
// Map {'yes' => true, 'no' => false}
```

但是，有一种特殊情况，整个 `JSON` 就是一个数组，且每个数组成员本身，又是一个有两个成员的数组。这时，它可以一一对应地转为` Map`。这往往是 `Map` 转为数组 `JSON` 的逆操作。

```javascript
function jsonToMap(jsonStr) {
  return new Map(JSON.parse(jsonStr));
}

jsonToMap('[[true,7],[{"foo":3},["abc"]]]')
// Map {true => 7, Object {foo: 3} => ['abc']}
```



以上就是`Set`和`map`的相关知识，详细内容可以查看[阮一峰老师](https://es6.ruanyifeng.com/#docs/set-map)博客。

