## React Router

````react
import React from "react";
import { BrowserRouter as Router, Route, Link } from "react-router-dom";
import Index from './Pages/Index'
import List from './Pages/List'

function AppRouter() {
  return (
    <Router>
        <ul>
            <li> <Link to="/">首页</Link> </li>
            <li><Link to="/list/123">列表</Link> </li>
        </ul>
        <Route path="/" exact component={Index} />
        <Route path="/list/:id" component={List} /> //路由动态传值  
    </Router>
  );
}

export default AppRouter;
````

1. exact 精确匹配的意思，就是说要路径信息完全匹配成功才能跳转
2. 接收路由传值用``this.props.match.params.参数名``

### 重定向 

1. redirect

````react
import { Link , Redirect } from "react-router-dom";
<Redirect to="/home/" />
````

2. 直接在构造函数`constructor`中加入下面的重定向代码。

   ````react
    this.props.history.push("/home/");  
   ````



### NavLink 和Link

````
Link进行的是路由切换跳转，整个单页面已经切换，而且能知道指向的路径是否是一个有效的路由


<NavLink>是<Link>的一个特定版本，会在匹配上当前的url的时候给已经渲染的元素添加参数，组件的属性有
activeClassName(string)：设置选中样式，默认值为active
activeStyle(object)：当元素被选中时，为此元素添加样式
exact(bool)：为true时，只有当导致和完全匹配class和style才会应用
strict(bool)：为true时，在确定为位置是否与当前URL匹配时，将考虑位置pathname后的斜线
isActive(func)判断链接是否激活的额外逻辑的功能
````

### 嵌套路由

嵌套路由一般使用Route,类似于vue中的作为嵌套路由的渲染，可以直接通过固定路由进入某一局部，等同于局部切换



**总路由 AppRouter.js 文件**

注意：在使用exact的组件下面，子路由会出现问题（包括子路由不显示等）

````react
import React from 'react'
import { BrowserRouter as Router, Route, Link } from 'react-router-dom' //imrr
import Index from './Pages/index'
import Video from './Pages/video/Video'
import WorkPlace from './Pages/Workplace'
import './Pages/index.css'
function AppRouter() {
    return (
        <Router>
        <div className="mainDiv">
          <div className="leftNav">
              <h3>一级导航</h3>
              <ul>
                  <li> <Link to="/">博客首页</Link> </li>
                  <li><Link to="/video">视频教程</Link> </li>
                  <li><Link to="/workPlace">职场技能</Link> </li>
              </ul>
          </div>

          <div className="rightMain">
              <Route path="/"  exact component={Index} />
              <Route path="/video"   component={Video} /> //在使用exact的组件下面，子路由会出现问题
              <Route path="/workPlace"   component={WorkPlace} />
          </div>
        </div>
    </Router>
    )
}
export default AppRouter

````



**二级路由 Video.js文件**

````react
import React from 'react'
import {  Route, Link } from 'react-router-dom'
import './video.css'
import Reactpage from './ReactPage'
import Vue from './Vue'
import Flutter from './Flutter'

function Video(){
    return (
        <div>
            <div className="topNav">
                <ul>
                    <li>
                        <Link to="/video/reactpage">React教程</Link>
                    </li>
                    <li>
                        <Link to="/video/vue">Vue教程</Link>
                    </li>
                    <li>
                        <Link to="/video/flutter">Flutter教程</Link>
                    </li>
                </ul>
            </div>
            <div className="videoContent">
                <div>
                    <h3>视频教程</h3>
                </div>
                <Route path="/video/reactpage" component = {Reactpage}/> //主要靠Route
                <Route path="/video/vue" component = {Vue}/>
                <Route path="/video/flutter" component = {Flutter}/>
            </div>
        </div>
    )
}

export default Video;
````



[详细例子](https://github.com/webfamer/react-router-demo)

![mark](http://images.91miandan.top/blog/20200831/uHW6flclJh8I.jpg?imageslim)