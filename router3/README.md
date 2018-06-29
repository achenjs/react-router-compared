### 基本用法
```
import { Router, Route, hashHistory } from 'react-router';

React.render((
  <Router history={hashHistory}>
    <Route path="/" component={App}>
      <IndexRoute component={Dashboard} />
      <Route path="about" component={About} />
      <Route path="inbox" component={Inbox}>
        <Route path="/messages/:id" component={Message} />
      </Route>
    </Route>
  </Router>
), document.body)

//  router-config.js
const routeConfig = [
  { path: '/',
    component: App,
    indexRoute: { component: Dashboard },
    childRoutes: [
      { path: 'about', component: About },
      { path: 'inbox',
        component: Inbox,
        childRoutes: [
          { path: '/messages/:id', component: Message },
          { path: 'messages/:id',
            onEnter: function (nextState, replaceState) {
              replaceState(null, '/messages/' + nextState.params.id)
            }
          }
        ]
      }
    ]
  }
]

React.render(<Router routes={routeConfig} />, document.body)
```

### histroy 属性
* hashHistory
* browserHistory(推荐)
* createMemoryHistory

### 组件
* IndexRoute
```
// 访问'/'根路径时，默认加载某个组件
<Router>
    <Route path="/" component={ App } >
        // 设置IndexRoute后 访问'/'路径时，默认加载'Home'组件 
        <IndexRoute component={ Home }/> 
        <Route path="accounts" component={ Accounts }/>
        <Route path="statements" component={ Statements }/>
    </Route>
</Router>
```
* IndexRedirect
```
//  访问'/'根路径时，默认重定向到某个子组件
<Route path="/" component={ App }>
    // 设置IndexRedirect后，访问'/'路径时，默认重定向到'welcome'组件
    <IndexRedirect to="/welcome" /> // to
    <Route path="welcome" component={ Welcome }  /> 
    <Route path="about" component={ About } />
</Route>
```
* Redirect
```
//  由访问路由自动跳转到另一个路由
<Route path="inbox" component={ Inbox }>
    // 访问/inbox/messages/5，会自动跳转到/messages/5
    ＜Redirect from="messages/:id" to="/messages/:id" /> // to
</Route>
```
* Link
```
//  作用类似于<a>标签，生成一个链接，允许点击后跳转到另一个路由，可以接收Router的状态
render() {
    return <div>
        <ul role='nav'>
            // 点击跳转到 /about
            <li><Link
              to='/about'
               activeStyle={{color: 'red'}}activeClassName="repos">About</Link></li> // to
            <li><Link to='/repos'>Repos</Link></li>
        </ul>
    </div>
}
```

### 按需加载
Route 可以定义 getChildRoutes，getIndexRoute 和 getComponents 这几个函数。它们都是异步执行，并且只有在需要时才被调用。我们将这种方式称之为 “逐渐匹配”。 React Router 会逐渐的匹配 URL 并只加载该 URL 对应页面所需的路径配置和组件。
![图呢？](/images/router-split3.png)

### 路由钩子
1. onEnter 进入路由时触发 场景：认证权限
2. onLeave 离开路由时触发 场景：跳出一个提示框，要求用户确认是否离开
