# React-Redux
在react中使用redux并不一定要使用React-Redux，可以直接使用redux。

#### UI组件和容器组件
React-Redux将所有组件分为UI 组件和容器组件。
UI组件只负责UI表示，不带状态   
容器组件负责业务逻辑，带状态   

#### connect()
connect负责从UI组件，生成一个容器组件   
使用如下：   
```
    // containerComponent是生成的容器组件
    const containerComponent = connect(
      mapStateToProps,
      mapDispatchToProps
    )(UIComponent)
```
##### mapStateToProps
输入逻辑：建立state到prop的映射关系
第一个参数总是state对象，第二个参数代表容器组件的props对象（可选）
mapStateToProps会订阅 Store，每当state更新的时候，就会自动执行，重新计算 UI 组件的参数，从而触发 UI 组件的重新渲染。
connect方法可以省略mapStateToProps参数，那样的话，UI 组件就不会订阅Store，就是说 Store 的更新不会引起 UI 组件的更新。

##### mapDispatchToProps
输出逻辑：建立 UI 组件的参数到store.dispatch方法的映射
第一个参数总是dispatch对象，第二个参数代表容器组件的props对象（可选）
UI组件调用定义的方法时，需要把对应方法传进去（参数同名）。

#### <Provider>
用来帮容器组件拿到state

#### 简单的例子
my-reactreduxapp\src\actions\actions.js
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

my-reactreduxapp\src\reducers\reducers.js
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

my-reactreduxapp\src\components\App.js
```
    import React from 'react'
    import PropTypes from 'prop-types'

    const Button = ({onClick}) => {
        return (
            <div>
                <button onClick={onClick}>add</button>
            </div>
        );
    }

    const App = ({todos, onClickButton}) => {
        return (
            <div>
                <div><h3>React-Redux Test</h3></div>
                <div>
                    <ul>
                        {todos.map( todo => 
                            <li>{todo.text}</li>
                        )}
                    </ul>
                    <Button onClick={() => onClickButton('XXXX')}></Button>
                </div>
            </div>
        );
    }

    App.propTypes = {
        todos: PropTypes.arrayOf(PropTypes.shape({
          completed: PropTypes.bool.isRequired,
          text: PropTypes.string.isRequired
        }).isRequired).isRequired,
        onClickButton: PropTypes.func.isRequired
    }

    export default App;
```

my-reactreduxapp\src\containers\AppContainer.js
```
    import { addTodo } from '../actions/actions'
    import { connect } from 'react-redux'
    import App from '../components/App'

    const mapStateToProps = state => {
        return {
            todos: state.todos
        }
    }

    const mapDispatchToProps = dispatch => {
        return {
            onClickButton: text => {
                console.log('click');
                dispatch(addTodo(text))
            }
        }
    }

    export default connect(
        mapStateToProps, 
        mapDispatchToProps
    )(App)
```

my-reactreduxapp\src\index.js
```
    import { createStore, compose } from 'redux'
    import React from 'react'
    import ReactDOM from 'react-dom'
    import { todoApp } from './reducers/reducers'
    import App from './containers/AppContainer'
    import { Provider } from 'react-redux'

    // 解决在使用Chrome redux插件“No store found”问题
    const enhancers = compose(
        window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
        );
    // const store = createStore(todoApp)
    const store = createStore(todoApp, window.STATE_FROM_SEVER, enhancers)

    ReactDOM.render(
        <Provider store={store}>
            <App />
        </Provider>,
        document.getElementById('root')
    );
```
