# React

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
