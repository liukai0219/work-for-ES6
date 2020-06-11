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

key
key 是 React 中一个特殊的保留属性（还有一个是 ref，拥有更高级的特性）。当 React 元素被创建出来的时候，React 会提取出 key 属性，然后把 key 直接存储在返回的元素上。虽然 key 看起来好像是 props 中的一个，但是你不能通过 this.props.key 来获取 key。React 会通过 key 来自动判断哪些组件需要更新。组件是不能访问到它的 key 的。
每次只要你构建动态列表的时候，都因该指定一个合适的 key。
如果你没有指定任何 key，React 会发出警告，并且会把数组的索引当作默认的 key。但是如果想要对列表进行重新排序、新增、删除操作时，把数组索引作为 key 是有问题的。显式地使用 key={i} 来指定 key 确实会消除警告，但是仍然和数组索引存在同样的问题，所以大多数情况下最好不要这么做。
组件的 key 值并不需要在全局都保证唯一，只需要在当前的同一级元素之前保证唯一即可。


Maximum update depth exceeded问题
this.props.onClick():会立即调用
this.props.onClick：不会立即调用
() => this.props.onClick()：不会立即调用
在render里面使用this.props.onClick()，并且onClick方法会修改state时，会陷入死循环。

### [JSX (JavaScript XML)](https://zh-hans.reactjs.org/docs/jsx-in-depth.html)
#### 语法糖
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
#### React 必须在作用域内
由于 JSX 会编译为 React.createElement 调用形式，所以 React 库也必须包含在 JSX 代码作用域内。   
即一定要引入react：`import React from 'react'`

#### 用户定义的组件必须以大写字母开头
以小写字母开头的元素代表一个 HTML 内置组件，比如 <div> 或者 <span> 会生成相应的字符串 'div' 或者 'span' 传递给 React.createElement（作为参数）。大写字母开头的元素则对应着在 JavaScript 引入或自定义的组件，如 <Foo /> 会编译为 React.createElement(Foo)。

#### 在运行时选择类型
不能把通用表达式作为React元素类型。如果想要动态决定元素类型，可以先把表达式的结果赋给大写字母开头的变量。
```
import React from 'react';
import { PhotoStory, VideoStory } from './stories';

const components = {
  photo: PhotoStory,
  video: VideoStory
};

function Story(props) {
  // 正确！JSX 类型可以是大写字母开头的变量。
  const SpecificStory = components[props.storyType];
  return <SpecificStory story={props.story} />;
}
```

#### Props 默认值为 “True”
不建议不传递 value 给 prop，因为这可能与 ES6 对象简写混淆，{foo} 是 {foo: foo} 的简写，而不是 {foo: true}。这样实现只是为了保持和 HTML 中标签属性的行为一致。

#### 属性展开
展开运算符 ... 
```
        const obj1 = {name:'Tom', age:'18', address:'abc'};
        const obj2 = {...obj1};
        const obj3 = {...obj1, name:'John'}; // 改变项目值
        const obj4 = {...obj1, name:'Kim', age:'20', address:'asd'}; // 改变项目值
        const obj5 = {...obj1, ...obj3, ...obj4};
        const obj6 = {...obj1, gender:'male'}; // 增加项目
        console.log(obj1); // {name: "Tom", age: "18", address: "abc"}
        console.log(obj1); // {name: "Tom", age: "18", address: "abc"}
        console.log(obj1); // {name: "John", age: "18", address: "abc"}
        console.log(obj1); // {name: "Kim", age: "20", address: "asd"}
        console.log(obj1); // {name: "Kim", age: "20", address: "asd"}
        console.log(obj1); // {name: "Tom", age: "18", address: "abc", gender: "male"}
        
        // 替换项目
        const Button = props => {
          const { kind, ...other } = props;
          const className = kind === "primary" ? "PrimaryButton" : "SecondaryButton";
          return <button className={className} {...other} />;
        };
```
#### JSX 中的子元素
包含在开始和结束标签之间的 JSX 表达式内容将作为特定属性 props.children 传递给外层组件。
布尔类型、Null 以及 Undefined 将会忽略
```
        <div />
        <div></div>
        <div>ABC{false}ABC</div>
        <div>ABC{null}ABC</div>
        <div>ABC{undefined}ABC</div>
        <div>ABC{true}ABC</div>
        //生成的HTML如下
        <div></div>
        <div></div>
        <div>ABCABC</div>
        <div>ABCABC</div>
        <div>ABCABC</div>
        <div>ABCABC</div>
```
可以用来根据条件判断是否渲染某元素    
{showHeader && <Header />}   
showHeader为true时才渲染   
数字 0，仍然会被 React 渲染,如下面。需要把`props.messages.length`改成`props.messages.length > 0`
```
<div>
  {props.messages.length &&
    <MessageList messages={props.messages} />
  }
</div>
```
