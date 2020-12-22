# REACT

个人学习 react 的一些记录，不定时更新。

---

## 目录

- [react-router-dom react 路由](#react-router-dom)

  - [react-router 组件三部分](#React-Router中组件主要分为三部分)
  - [React-Router 的 Hooks 使用](#React-Router的Hooks使用)

- [HTML 两种渲染方式](#HTML-两种渲染方式)

  - [SSR-服务器渲染/后端渲染](#SSR-服务器渲染/后端渲染)
  - [CSR-客户端渲染/前端渲染](#CSR-客户端渲染/前端渲染)

- [Mobx](https://github.com/shanezzzz/react-doc/blob/master/MOBX.md)

- [Webpack](https://github.com/shanezzzz/react-doc/blob/master/WEBPACK.md)

- [less](https://github.com/shanezzzz/react-doc/blob/master/LESS.md)

---

## react-router-dom

### React-Router 中组件主要分为三部分

- 路由器：[BrowserRouter](#BrowserRouter) 和 [HashRouter](#HashRouter)
- 路由匹配器：[Router](#Router) 和 [Switch](#Switch)
- 导航：[Link](#Link)、[NavLink](#NavLink) 和 [Rediert](#Rediert)

### React-Router 的 Hooks 使用

- [useHistory](#useHistory)
- [useLocation](#useLocation)
- [useParams](#useParams)
- [useRouteMatch](#useRouteMatch)

---

#### **BrowserRouter**

- 路由器。常规 URL 路径，没有#号在里面。使用浏览器中 History API 处理 URl，创建一个类似 https://www.google.com ,这样的真实地址。这种需要服务器处理 URL，因为每次页面的跳转服务器都会接受到来至来 URL 的请求。

#### **HashRouter**

- 路由器。将当前位置存储在 URL 的哈希部分中，带#号。如：google/#/abc。哈希不会发送到服务器，所以不需要服务器做配置。

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

- 路由匹配器。相当于 switch case 语法，找到第一个匹配的属性后，立即进入条状，停止再继续搜索。
- 注意先后顺序，会和 switch 语法一样，至上而下开始搜索匹配。同时还会匹配 router 是否开启 exact。
- 如果 path 填的是`/`，则说明匹配所有。注意要放到最后。

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

- 相当于 a 标签`<a href="./index">`。

```shell
import { Link } from 'react-router-dom';

const link = (
    <Link to="./index" />
)
```

#### **NavLink**

- 作用和 link 相同，但与别与 link
  - Link: 用于导航站点上的不同路由
  - NavLink: 是一种特殊 Link，用于将样式属性添加到当前路由。可通过设置`activeStyle`和`activeClassName`属性进行样式的添加

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

- 返回当前 URL 的 location 对象，类似 hook 中的`useState`。只要是 URL 改变了，就会返回一个新的位置。

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

- 返回 URL 参数的键/值对的对象,使用它来访问当前`<Router>`的`match.params`。

#### **useRouteMatch**

- 与`<Router>`相同的方式匹配当前 URL。它主要用于在无需实际呈现`<Route>`即可访问匹配数据的需求。

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

---

## HTML-两种渲染方式

### SSR-服务器渲染/后端渲染

1. 前端请求一个地址 url
2. 服务器接受 url 请求，并在数据库拿到相应的数据
3. 服务器使用模版引擎，将这些数据渲染成 html
4. 将 html 返回给前端
5. 前端再继续发送其他 ajax 请求

   #### _优点_

   - 客户端耗时少，因为在服务器渲染，拼接好再发送给客户端。客户端不需要先下载一堆 css/js 文件才看到页面
   - 利于 SEO
   - 减少数据库查询。因为是后端生成静态文件，即后端生成缓存片段，这样可以减少数据库查询

   #### _缺点_

   - 不利于前后端分离，开发效率低。
   - 服务器压力大，因为所有渲染、拼接都在服务器上进行。

### CSR-客户端渲染/前端渲染

1. 前端请求一个地址 url
2. 服务器接受 URL，然后把相应的 Html 直接返回给前端
3. 前端继续发送其他 ajax 请求，服务器拿到请求，通过数据库获取对应的数据
4. 前端将这些数据动态渲染成页面

   #### _优点_

   - 前后端分离，加大开发效率
   - 局部刷新，无需每次都进行完整页面请求
   - 强体验。前端渲染可以做成更好的用户体验页面，更炫酷的页面
   - 节约服务器成本。

   #### _缺点_

   - 不利于 SEO
   - 前端首屏加载慢

   ***
