## React-Router

#### React-Router与React-Router-Dom,React-Router-Native的关系
React-Router：为 React Router 应用提供了核心的路由组件和函数    
React-Router-Dom：基于react-router，加入了在浏览器运行环境下的一些功能   
React-Router-Native：基于react-router，加入了react-native运行环境下的一些功能。   
基于浏览器环境的开发，只需要导入React-Router-Dom。   

#### 主要组件
路由：`<BrowserRouter> `, `<HashRouter>`
  `<BrowserRouter>`：使用正常的URL路径，使用HTML5 history API，支持location.key或location.state
  `<HashRouter>`：不支持location.key或location.state，URL形式为http://example.com/#/your/page
  需要把路由组件放到顶层。
路由匹配： `<Route>` , `<Switch>`
  switch会自上而下的查找route的path，只要找到就会忽略其他的，所以要把匹配范围最小的放到上方。如果没有找到，会返回null。
  path的匹配是先头一致，而不是完全一致，所以[/]可以匹配所有路径。
  使用exact可以设置为精确匹配，[/]将只匹配[/]，而不会匹配所有。
导航： `<Link>`, `<NavLink>`, `<Redirect>`
  NavLink可以设置activeClassName属性，当link处于active状态时应用该属性。
  Redirect重定向。
    
    
#### 静态路由和动态路由
静态路由：在应用初始化时，声明路由。运行时，路由配置无法改变。   
  使用静态路由的框架：Rails, Express, Ember, Angular,React Router pre-v4   
动态路由：在页面渲染时路由。根据运行时的状态路由。   

#### 传值
方式1：路由传值
```
  // 传值
  <Link to="/name/tom">tom</Link>
  // route组件的path属性，通过`:name`形式接收参数
  <Route path="/name/:name" component={Child}/></Route>

  function Child() {
    // 取值
    // 方式1 使用useParams()获取参数
    let { name } = useParams();
    // 方式2 从props里面取值
    let name = props.match.params?.name
  }
```

方式2:URL传值
```
  // 传值
  // 方式1
  <Link to='/page03?name=tom'> page03 </Link>
  // 方式2
  const pathPage03 = {
        pathname: '/page03',
        search:'name=tom'
    };
    <Link to={pathPage03}> page03 </Link>
    或者
    props.history.push(pathPage03);
  
  // 取值
  // 方式1
  let search = useLocation().search; // ?name=tom
  
  // 方式2
  let name = new URLSearchParams(useLocation().search).get("name"); // tom
```

方式3：state传值
```
  // 传值
  const pathPage02 = {
      pathname: '/page02',
      query: {
          name: 'tom'
      },
      state: {
          age: '12'
      }
  };
  <Link to={pathPage02}> page02 </Link>
  或者
  props.history.push(pathPage02);
  
  // 取值
  // 方式1
  let age = useLocation().state.age; // 12
  
  // 方式2
  let age = this.props.location.state.age; // 12
```


#### 例子

my-reactrouterapp\src\index.js
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

my-reactrouterapp\src\containers\AppContainer.js
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

my-reactrouterapp\src\components\App.js
```
  import React from 'react'
  import { Link, Route, Switch, BrowserRouter as Router, useLocation, Redirect } from 'react-router-dom'
  import { Page01 } from './Page01'
  import { Page02 } from './Page02'
  import { Page03 } from './Page03'



  const App = (props) => {   

      //  pathPage02为Link to属性的参数，包含以下属性
      //  pathname：要跳转的页面,不能加参数，参数放到search里
      //  search：字符串，接在url后面，URL传参
      //  state：对象，刷新页面不会清掉
      //  query：刷新页面会清掉,不会添加到url后面（以前的版本会，现在用search代替）
      const pathPage02 = {
          pathname: '/page02',
          query: {
              name: 'tom'
          },
          search:'sex=female',
          state: {
              age: '12'
          }
      };
      console.log('App props:')
      console.log(props)
      return (
          <Router>
              <div>
                  <div><h2>React-Router Test</h2></div>
                  <div>
                      <div>
                          <Link to='/'> Home </Link> | 
                          <Link to='/page01/page02'> page01 </Link> | 
                          {/* to属性的值，可以是字符串（要跳转页面的路径），比如：/page01  也可以是对象，比如pathPage02 ,还能是个方法，比如：location => `${location.pathname}?sort=name`*/}
                          {/* replace ： 添加该属性，会把history替换，而不是添加 */}
                          <Link to={pathPage02}> page02 </Link> | 
                          <Link to='/page03?name=tom'> page03 </Link> | 
                          <Link to='/redirect'> redirect to 404 </Link> | 
                          <Link to='/404'> page404 </Link>
                      </div>
                      <div>
                          <Switch>
                              {/* exact：精确匹配，必须要路径一样（不区分大小写） */}
                              <Route exact path='/'>
                                  <div>
                                      <h4>Welcome!</h4>
                                  </div>
                              </Route>
                              {/* <Route path='/Page01'> 默认是先头匹配'/Page01'可以匹配到'/page01/page02'*/}
                              <Route path='/Page01'>
                                  <Page01 />
                              </Route>
                              <Route path='/Page02' component={Page02}>
                                  {/* 不使用component属性设置路由的组件的话，在子组件中的props中无法获得location，history */}
                                  {/* <Page02 /> */}
                              </Route>
                              <Route path='/Page03' component={Page03}>
                                  {/* <Page03 /> */}
                              </Route>
                              {/* <Redirect from='/redirect' to='/404'></Redirect> */}
                              <Route path='/redirect'>
                                  <Redirect to='/404'></Redirect>
                              </Route>
                              {/* 放到最后，应对无法匹配的情况 */}
                              <Route path='*'>
                                  <NoMatch></NoMatch>
                              </Route>
                          </Switch>
                      </div>
                  </div>
              </div>
          </Router>
      );
  }

  function NoMatch() {
      let location = useLocation();

      return (
        <div>
          <h3>
            No match for <code>{location.pathname}</code>
          </h3>
        </div>
      );
  }

  export default App;
```

my-reactrouterapp\src\components\Page01.js
```
  import React from 'react'
  import { Link, Route, Switch, useRouteMatch, useParams } from 'react-router-dom'
  import { Page01X } from './Page01X'

  const Page01 = () => {
      let match = useRouteMatch();
      console.log('useRouteMatch().url:' + match.url); // useRouteMatch().url:/page01
      console.log('useRouteMatch().path:' + match.path); // useRouteMatch().path:/page01

      return (
          <div>
              <h3>This is page01!</h3>
              <div>
                  <Link to={`${match.url}/page01-1`}> page01-1 </Link>
                  <Link to={`${match.url}/page01-2`}> page01-2 </Link>
                  <Link to={`${match.url}/page01-3`}> page01-3 </Link>
              </div>
              <div>
                  <Switch>
                      {/* 路由传参:URL的一部分作为参数pageId传递到<Page01X />，通过"let { pageId } = useParams();"获得参数 */}
                      <Route path={`${match.url}/:pageId`} component={Page01X}>
                          {/* <Page01X /> */}
                      </Route>
                      <Route path={match.path}>
                          <div>
                              <h4>Welcome to page01!</h4>
                          </div>
                      </Route>
                  </Switch>
              </div>
          </div>
      );
  }

  export {Page01}
```
my-reactrouterapp\src\components\Page01X.js
```
  import React from 'react'
  import { useParams } from 'react-router';

  const Page01X = (props) => {
      console.log(props)
      let { pageId } = useParams();
      return (
          <div>
              {/* 路由传参，取值方式 */}
              <h3>This is {pageId}!</h3>
              <div>This is {props.match.params?.pageId}!</div>
              <div>{props.location.state?.from ? 'from ' + props.location.state.from : ''}</div>
          </div>
      );
  }

  export {Page01X}
```
my-reactrouterapp\src\components\Page02.js
```
  import React from 'react'

  const Button = (props) => {
      return (
          <div>
              <button onClick={() => onhandleClick(props)}>Go to Page01X</button>
          </div>
      );
  }

  const onhandleClick = (props) => {
      console.log(props)
      const path = {
          pathname: '/page01/page01-1',
          state: {
              from: 'page02'
          }
      }
      props.history.push(path);
  }

  class Page02 extends React.Component{
      constructor(props) {
          super(props);
      }

      render() {
          console.log(this.props);
          // class类型的组件，可以从props中获得location
          // 函数组件，可以使用hook，获得location
          let location = this.props.location;

          return (
              <div>
                  <h3>This is page02!</h3>
                  location<br/>
                  <hr></hr>
                  key:{location.key}<br/>
                  pathname:{location.pathname}<br/>
                  hash:{location.hash}<br/>
                  search:{location.search}<br/>
                  state.age:{location.state?.age}<br/>
                  query.name:{location.query?.name}<br/>
                  <hr></hr>
                  <div>
                      {/* 需要把props传下去，不然无法获得location */}
                      <Button {...this.props}></Button>
                  </div>
              </div>
          );
      }
  }

  export {Page02}
```

my-reactrouterapp\src\components\Page03.js
```
  import React from 'react'
  import { useLocation } from "react-router-dom";

  function useQuery() {
      // search的值是?+&连接的键值对形式，需要进行解析
      return new URLSearchParams(useLocation().search);
  }

  const Page03 = (props) => {
      let query = new URLSearchParams(useLocation().search);
      console.log(props);
      // let location = props.location;
      // 函数组件，可以使用hook，获得location,也可以向上面从props里面拿
      let location = useLocation();

      return (
          <div>
              <h3>This is page03!</h3>
              location<br/>
              <hr></hr>
              pathname:{location.pathname}<br/>
              search:{location.search}<br/>
              url:{new URLSearchParams(useLocation().search).get("name")}<br/>
          </div>
      );
  }

  export {Page03}
```




