# React


函数组件
如果组件只包含一个 render 方法，并且不包含 state，那么可以定义成一个函数，这个函数接收 props 作为参数，然后返回需要渲染的元素。

不可变性
pure components

构造函数
在 JavaScript class 中，每次你定义其子类的构造函数时，都需要调用 super 方法。因此，在所有含有构造函数的的 React 组件中，构造函数必须以 super(props) 开头。

组件相互通讯
当你遇到需要同时获取多个子组件数据，或者两个组件之间需要相互通讯的情况时，需要把子组件的 state 数据提升至其共同的父组件当中保存。之后父组件可以通过 props 将状态数据传递到子组件当中。这样应用当中所有组件的状态数据就能够更方便地同步共享了。

state
state 对于每个组件来说是私有的，因此我们不能直接通过 Square 来更新 Board 的 state。


Maximum update depth exceeded问题
this.props.onClick():会立即调用
this.props.onClick：不会立即调用
() => this.props.onClick()：不会立即调用
在render里面使用this.props.onClick()，并且onClick方法会修改state时，会陷入死循环。

JSX (JavaScript XML)
JSX 是一个看起来很像 XML 的 JavaScript 语法扩展。
可以在js中编写类XML的内容，同时在类XML内容中可以使用js(需要使用大括号括起来)。
实际上，JSX 仅仅只是 React.createElement(component, props, ...children) 函数的语法糖。

```
<div className="shopping-list">
        <h1>Shopping List for {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
```
上面的代码会被编译成下面的样子
```
return React.createElement('div', {className: 'shopping-list'},
  React.createElement('h1', /* ... h1 children ... */),
  React.createElement('ul', /* ... ul children ... */)
);
```
