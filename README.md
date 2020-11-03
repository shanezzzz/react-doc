# REACT
个人学习react的一些记录，不定时更新。

*****
## 目录
* [react-router-dom react路由](#react-router-dom)
    - [react-router组件三部分](#React-Router中组件主要分为三部分)
    - [React-Router的Hooks使用](#React-Router的Hooks使用)

******

## react-router-dom

### React-Router中组件主要分为三部分
- 路由器：[BrowserRouter](#BrowserRouter) 和 [HashRouter](#HashRouter)
- 路由匹配器：[Router](#Router) 和 [Switch](#Switch)
- 导航：[Link](#Link)、[NavLink](#NavLink) 和 [Rediert](#Rediert)

### React-Router的Hooks使用
- [useHistory](#useHistory)
- [useLocation](#useLocation)
- [useParams](#useParams)
- [useRouteMatch](#useRouteMatch)

-----

#### **BrowserRouter**
- 路由器。常规URL路径，没有#号在里面。使用浏览器中History API处理URl，创建一个类似 https://cn.pornhub.com/view_video.php?viewkey=ph5ef8cf8e75953 (别打开),这样的真实地址。这种需要服务器处理URL，因为每次页面的跳转服务器都会接受到来至来URL的请求。

#### **HashRouter**
- 路由器。将当前位置存储在URL的哈希部分中，带#号。如：google/#/abc。哈希不会发送到服务器，所以不需要服务器做配置。

使用路由器，一般会将最顶级的元素给包住，确保在该路由器里面，如：
```shell
import React from 'react';
import ReactDOM from 'react-dom';
import { BrowserRouter } from 'react-router-dom';

function App() {
    return <h1>Don't Holle world</h1>;
}

ReactDOM.render(
    <BrowserRouter>
        <App />
    </BrowserRouter>
);
```


#### **Router**
- 路由匹配器。具有渲染方法的路由组件，包含三个属性：
    - path: 路由的匹配规则，路径
    - exact: 是否精准匹配
    - component: 需要渲染的组件，必填

```shell
import { Router } from 'react-router-dom';
import Index from './index';

cosnt routing = (
    <Router path="/index" exact component={Index} />
)
```

#### **Switch**
- 路由匹配器。相当于switch case语法，找到第一个匹配的属性后，立即进入条状，停止再继续搜索。
- 注意先后顺序，会和switch语法一样，至上而下开始搜索匹配。同时还会匹配router是否开启exact。
- 如果path填的是`/`，则说明匹配所有。注意要放到最后。

```shell
import { Router, Switch } from 'react-router-dom';
import Index from './index';
import Login from './login';
import 404 from './404';

const routing = (
    <Switch>
        <Router path="/index" component={Index}>
        <Router path="/login" component={Login}>
        <Router path="/" component={404}>
    </Switch>
)
```

#### **Link**
- 相当于a标签`<a href="./index">`。

```shell
import { Link } from 'react-router-dom';

const link = (
    <Link to="./index" />
)
```

#### **NavLink**
- 作用和link相同，但与别与link
    - Link: 用于导航站点上的不同路由
    - NavLink: 是一种特殊Link，用于将样式属性添加到当前路由。可通过设置`activeStyle`和`activeClassName`属性进行样式的添加

```
import { NavLink } from 'react-router-dom';

const NavLink = (
    <NavLink to="./index" activeStyle={{ color: '#000' }} activeClassName="link" />
)

```

#### **Rediert**
- 重定向

```
import { Rediert } from 'react-router-dom';
import Index from './index';

const Rediert =  (
    <Rediert path="./index" component={Index} />
)
```

#### **useHistory**
- 导航的历史记录

```
import { useHistory } from 'react-router-dom';

const Example = () => {
    const history = useHistory();
    history.push('/index'); // 可实现跳转
}
```
#### **useLocation**
- 返回当前URL的location对象，类似hook中的`useState`。只要是URL改变了，就会返回一个新的位置。

```
import { useLocation } from 'react-router-dom';

const Location = () => {
    const location = useLocation();
    useEeffect(() => {
        console.log(location);
    }, [location]);
}
```

#### **useParams**
- 返回URL参数的键/值对的对象,使用它来访问当前`<Router>`的`match.params`。

#### **useRouteMatch**
- 与`<Router>`相同的方式匹配当前URL。它主要用于在无需实际呈现`<Route>`即可访问匹配数据的需求。

```
import { Route } from "react-router-dom";

function BlogPost() {
  return (
    <Route
      path="/blog/:slug"
      render={({ match }) => {
        // Do whatever you want with the match...
        return <div />;
      }}
    />
  );
}
```
可替换为
```
import { useRouteMatch } from "react-router-dom";

function BlogPost() {
  let match = useRouteMatch("/blog/:slug");

  // Do whatever you want with the match...
  return <div />;
}
```
