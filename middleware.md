## 中间件
中间件实现是通过对`store.dispatch`进行重定义,在发出action的前后添加功能。   

### applyMiddlewares
作用是将所有中间件组成一个数组，依次执行。

### redux-thunk 异步处理
store.dispatch原本只能接收一个对象作为参数，添加redux-thunk中间件后，可以接受函数。   
在发出action之前，会判断是否是函数，是的话会先传入dispatch, getState并执行。所以传入dispatch的函数里面是可以获得dispatch和state的。   

redux-thunk的源码如下
```
    function createThunkMiddleware(extraArgument) {
      return ({ dispatch, getState }) => next => action => {
        if (typeof action === 'function') {
          return action(dispatch, getState, extraArgument);
        }

        return next(action);
      };
    }

    const thunk = createThunkMiddleware();
    thunk.withExtraArgument = createThunkMiddleware;

    export default thunk;
```


### redux-logger 日志中间件

### redux-promise
