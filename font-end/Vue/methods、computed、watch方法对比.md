在学习`Vue`的过程中，我们常常会因为这些属性的相似性而变得混乱，甚至可能会因此丧失学习兴趣，那么我们今天就来看看这几个属性的联系以及它们的区别。

首先我们来简单看看方法（methods）、计算属性（computed）以及侦听属性（watch）的使用。

#  计算属性、方法以及侦听属性的区别和联系



在`Vue`中,实现一个需求，我们可以通过使用`methods`方法实现，也可以使用`computed`计算属性来实现。

##  计算属性和方法

```javascript
//在这两段代码中，所实现的功能是完全相同的
computed: {
	fullName: function() {
 		return this.firstName + '  ' + this.lastName;
	}
}
```

```javascript
 methods: {
     getfullName: function() {
         return this.firstName + '  ' + this.lastName;
	 }
 }
```

那么既然代码实现的功能相同我们为什么要使用新的属性呢，而不去使用我们经常需要使用的methods方法呢。

我们现在来看看在渲染用户界面时候发生了什么，我们使用一个小小的`console.log()`来看看渲染时候发生了什么。

![使用两种方法不同的调用情况](https://img-blog.csdnimg.cn/2020070821004228.png)

在渲染时候，我们可以看到调用了计算属性只需要一次，而调用方法需要四次。

说明做多少次重复的运算，那么方法函数将会被调用多少次，相反计算属性仅仅只会调用一次，说明在性能方面，使用计算属性可优化性能。

> 对于相当大的程序，我们使用计算属性，会更好的优化代码，使得代码变得简单，高效。
>
> 上面的一切都依赖于计算属性的一个特点：计算属性会根据依赖他们进行缓存，并且不会轻易改变，只有计算属性的相关依赖的变量发生变化时候才会发生变化。

## 侦听属性 

在`Vue`中，计算属性是一个很方便的方法，但是有时候我们也需要一些自定义的方法。

这时候，侦听属性`Wacth`就显得更加方便一些，我们可以使用它来监听`data`值的变化。

~~~javascript
[[key:string]:String|Function|Object]
~~~

在`Watch`中，键是一个字符串类型，它是被观测的对象，而值可以是字符串、函数，也可以是对象。

在`data`中我们保存的可能是`Number`,`String`,`Boolean`,`Object`,`Null`等。

###  深度监听

访问除了`Object`以外的数据时候我们通常可以直接访问，访问对象中的数据的时候我们需要借助一个属性`deep：true`。

~~~javascript
 var vm = new Vue({
        data: {
          a: 1,
          b: 2,
          c: {
            name: "JohnZhu"
          }
        },
        watch: {
          a: function (val, oldVal) {
            console.log('new a: %s, old a: %s', val, oldVal)
          },
          // 方法名
          b: 'someMethod',

          // 深度 watcher， 检测到变化，并打印出c.name变化前后的结果
          // 'c.name': {
          //   handler: function (val, oldVal) { 
          //     console.log('new c: %s, old c: %s', val, oldVal);
          //   },
          //   deep: true
          // },

          // 报错 必须用c.name，否则在data下不能直接找到name
          // name: function () {
          //   console.log('new c: %s, old c: %s', val, oldVal);
          // }

          // 报错，键值必须是一个字符串，所以用引号括起来
          // c.name: {
          //   handler: function (val, oldVal) {
          //     console.log('new c: %s, old c: %s', val, oldVal);
          //   },
          //   deep: true
          // }

          // 这里未检测到变化
          // c : {
          //   handler: function (val, oldVal) { 
          //     console.log('new c: %s, old c: %s', val, oldVal);
          //   },
          //   deep: false
          // },
         
          // 成功检测到变化 
          // c : {
          //   handler: function (val, oldVal) { 
          //     console.log('new c: %s, old c: %s', val, oldVal);
          //   },
          //   deep: true
          // },

          // 检测不到变化，因为参数 deep 的默认值是false
          // c : {
          //   handler: function (val, oldVal) { 
          //     console.log('new c: %s, old c: %s', val, oldVal);
          //   },
          // },
        },
        methods: {
          someMethod: function () {
            alert("b is changed");
          }
        }
      })
      vm.a = 2; // new: 2, old: 1
      vm.b = 666; // alert 666
      vm.c.name = "HTT";
~~~

`Wacth`属性可以监测到`a`,`b`值的改变，从而使用对应的函数。

###  用字符串表示对象的属性调用

~~~
new Vue({
	el:"#app",
	data:{
	a:1,
	blog:{
		catories:'qweqweqw',
		aja:[]
		}
	},
	Wacth:{
		'blog.aja'(newVal,oldVal){
			consloe.log('new:${newVal},old:${oldVal}')，
			deep：true
			
		}
	}
});
~~~

在这个例子中，我们使用了深度监测访问到了`blog`中的数据，也使用了字符串作为键去访问。





