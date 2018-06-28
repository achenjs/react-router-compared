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
```

### histroy 属性
* hashHistory
* browserHistory(推荐)
* createMemoryHistory

### 路由钩子
1. onEnter 进入路由时触发 场景：认证权限
2. onLeave 离开路由时触发 场景：跳出一个提示框，要求用户确认是否离开
