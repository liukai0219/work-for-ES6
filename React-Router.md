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
方式1：url传值
```
  <Link to="/name/tom">tom</Link>
  // route组件的path属性，通过`:name`形式接收参数
  <Route path="/name/:name" /><Child /></Route>

  function Child() {
    // 使用useParams()获取参数
    let { name } = useParams();
  }
```
#### history


































