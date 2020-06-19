## Redux
#### redux和react
redux和react并没有关系，你可以配合Angular, Ember, jQuery等构建redux应用。

#### 什么情况下需要使用redux
Redux 的适用场景:多交互、多数据源
用户的使用方式复杂
不同身份的用户有不同的使用方式（比如普通用户和管理员）
多个用户之间可以协作
与服务器大量交互，或者使用了WebSocket
View要从多个来源获取数据

从组件角度看
某个组件的状态，需要共享
某个状态需要在任何地方都可以拿到
一个组件需要改变全局状态
一个组件需要改变另一个组件的状态

#### 设计思想
（1）Web 应用是一个状态机，视图与状态是一一对应的。   
（2）所有的状态，保存在一个对象里面。   

#### store
单一的真实来源
应用的全局state都被存储在store里面,一个应用只能有一个store
通过createStore生成

#### state
state是只读的
state就像一个普通的对象，但没有setter，无法随意改变它的值，唯一改变state的方法就是发出一个action
store.getState()可以获得当前state

#### action
action也是一个普通的对象，描述了将要发生什么。
store.dispatch()发出action

#### reducer
reducer是个纯函数
关联state和action的方法就是reducer，reducer只进行state的计算并返回一个新的state。
对于大型应用，state十分庞大，reducer函数必然也会特别庞大，这是你需要把reducer进行拆分，拆分后的reducer使用combineReducers合并。

#### store.subscribe()
设置监听
当state有变化会执行，可以用来绑定render，重新渲染画面

#### 简单的例子
my-reduxapp\src\store\store.js
```
    import { createStore, compose } from 'redux'
    import { todoApp } from '../reducers/reducers'

    // 解决在使用Chrome redux插件“No store found”问题
    const enhancers = compose(
      window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
      );
    // const store = createStore(todoApp)
    const store = createStore(todoApp, window.STATE_FROM_SEVER, enhancers)

    export { store }
```
my-reduxapp\src\actions\actions.js
```
    const ADD_TODO = 'ADD_TODO'

    // action定义
    const addTodo = (text) => {
        return {
            type: ADD_TODO,
            text: text
        };
    }

    // 命名导出
    export {
        addTodo, 
        ADD_TODO
    }
```

my-reduxapp\src\reducers\reducers.js
```
    import { ADD_TODO } from '../actions/actions'

    const initialState = {
        todos: []
    }

    // 当进行了reducer分割时，使用combineReducers合成，redux会根据state的key查找对应子reducer
    // const todoAppReducer = combineReducers({reducer01, reducer02})

    // state = initialState  ES6引入的给参数赋默认值
    function todoApp(state = initialState, action) {
        switch (action.type) {
            case ADD_TODO:
                return Object.assign({}, state, {
                    todos: [
                        ...state.todos,
                        {
                            text: action.text,
                            completed: false
                        }
                    ]
                })
                // Object.assign(target, ...sources)
                // 将sources复制到target，并返回target，所以第一个参数是会被修改，state不能作为第一个参数
            default:
                // 默认返回当前state是很有必要的，用来处理未对应的action
                return state;
        }
    }

    export { todoApp };
```

my-reduxapp\src\components\App.js
```
    import React from 'react'
    import { addTodo } from '../actions/actions'
    import { store } from '../store/store'

    function onClick(text) {
        console.log(store.getState());
        store.dispatch(addTodo(text))
        console.log(store.getState());
    }

    const Button = ({onClick}) => {
        return (
            <div>
                <button onClick={onClick}>add</button>
            </div>
        );
    }

    const App = () => {
        return (
            <div>
                <div><h3>Redux Test</h3></div>
                <div>
                    <Button onClick={() => onClick('XXXX')}></Button>
                    <ul>
                        {store.getState().todos.map( todo => 
                            <li>{todo.text}</li>
                        )}
                    </ul>
                </div>
            </div>
        );
    }

    export { App }
```

my-reduxapp\src\index.js
```
    import React from 'react'
    import ReactDOM from 'react-dom'
    import { store } from './store/store'
    import { App } from './components/App'

    const render = () => ReactDOM.render(<App />, document.getElementById('root'))

    // 执行render方法，渲染页面
    render()

    // 注册监听，当state变更后，调用render方法，重新渲染页面
    // 不注册监听的话，state变更后，画面不会重新渲染
    store.subscribe(render);
```
