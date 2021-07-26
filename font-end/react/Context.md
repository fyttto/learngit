# Context

我们经常在组件之间传递值的时候会遇到一种情况——某一个值/某一组值是所有组件都需要的，比如，用户信息，深浅模式状态等等。

Context就提供了一个无需为每层组件手动添加Props，就能在组件树间进行数据传递的方法。

## API

### React.createContext

创建一个Context对象。当React渲染一个订阅了这个Context对象的组件，这个组件会从组件树中最近的那个匹配到的provider中读取当前的Context值。

```javascript
const myContext=React.createContext(defaultValue); 	
```

> 注：只有当组件树中的组件匹配不到provider的时候，defaultValue才会生效。undefined作为值传入到provider中的时候，defaultValue也不会生效。

### Context.provider

每一个Context对象都会都会返回一个Provide React组件，它允许消费组件订阅context的变化。

Provider接受一个value属性，传递给消费组件。一个Provider可以和多个消费组件有对应关系。

```javascript
<myContext.provider value={/* 某个值 */}>
```

它的值发生变化时候，它内部的所有消费组件都会重新渲染

