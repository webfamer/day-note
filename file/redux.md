## redux

需要使用redux的情况

1. 某个组件的状态，需要共享
2. 某个状态需要在任何地方都可以拿到
3. 一个组件需要改变全局状态
4. 一个组件需要改变另一个组件的状态

**Action Creator**

定义一个函数来生成Action,这个函数就叫 Action Creator

````javascript
const ADD_TODO = '添加TODO';
function addTodo(text){
    return {
        type:ADD_TODO,
        text
    }
}
const action = addTodo('Learn Redux')
````



### 创建redux的步骤

1.先建立store和reducer

在src目录先创建store文件夹，然后在文件夹下创建index.js,reducer.js，然后写入如下代码

**index.js**

````react
import { createStore } from 'redux' //引入createStore方法
import reducer from './reducer'
const store = createStore(reducer) //创建数据存储仓库
export default store //暴露出去
````

**reducer.js**

````react
const defaultSate = {} //默认数据
export default (state = defaultState,action) =>{
    return state
}
````

### 优化技巧

1. 把Action Types单独写入一个文件

   在`src/store`文件夹下面，新建立一个`actionTypes.js`文件，然后把Type集中放到文件中进行管理

   ````react
   export const  CHANGE_INPUT = 'changeInput'
   export const  ADD_ITEM = 'addItem'
   export const  DELETE_ITEM = 'deleteItem'
   ````

2. 把所有的`Redux Action`放到一个文件里进行管理。

   在`/src/store`文件夹下面，建立一个新的文件`actionCreators.js`，先在文件中引入上节课编写`actionTypes.js`文件。

````react
import {CHANGE_INPUT}  from './actionTypes'

export const changeInputAction = (value)=>({
    type:CHANGE_INPUT,
    value
})
````

然后就可以引入文件使用了

````reac
import {changeInputAction} from './store/actionCreatores'
````

````react
changeInputValue(e){
        const action = changeInputAction(e.target.value)
        store.dispatch(action)
    }
````

### 无状态组件

无状态组件就是一个函数，不继承任何类（class）,也不存在状态（state），所有性能比普通React组件好

### redux-thunk

使用场景：在`Dispatch`一个`Action`之后，到达`reducer`之前，进行一些额外的操作，就需要用到`middleware`（中间件）。在实际工作中你可以使用中间件来进行日志记录、创建崩溃报告，调用异步接口或者路由

**安装**

````react
npm install --save redux-thunk
````

**配置**

1.要使用中间件，就必须在redux引入``applyMiddleware``

````react
import { createStore , applyMiddleware } from 'redux' 
````

2.引入 redux-thunk 库

````react
import thunk from 'redux-thunk'
````

完整代码

````react
import { createStore , applyMiddleware ,compose } from 'redux'  //  引入createStore方法
import reducer from './reducer'    
import thunk from 'redux-thunk'

const composeEnhancers =   window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ ?
    window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({}):compose  //这里引用了增强函数compose

const enhancer = composeEnhancers(applyMiddleware(thunk))

const store = createStore( reducer, enhancer) // 创建数据存储仓库
export default store   //暴露出去
````

compose增强函数

把嵌套调用的函数写法简化成链式调用

[compose中对reduce的应用](https://segmentfault.com/a/1190000015801987)



### redux-saga

作用与redux-thunk一样



### React-redux

1.先正常安装使用Redux

2.安装React-redux，并引用其中的Provider和connect

``<Provider>``提供器

使用了这个组件后，组件里的所有其他组件都可以使用store,具体如下

````react
import React from 'react';
import ReactDOM from 'react-dom';
import TodoList from './TodoList'
//---------关键代码--------start
import { Provider } from 'react-redux'
import store from './store'
//声明一个App组件，然后这个组件用Provider进行包裹。
const App = (
    <Provider store={store}>
        <TodoList />
    </Provider>
)
//---------关键代码--------end
ReactDOM.render(App, document.getElementById('root'));
````

``connect``连接器

````react
import React, { Component } from 'react';
import store from './store'
import { connect } from 'react-redux'
const stateToProps = (state) => { //映射关系的制作
    return {
        inputValue: state.inputValue,
        list: state.list
    }
}

const dispatchToProps = (dispatch) => {//映射关系的制作
    return {
        inputChange(e) {
            let action = {
                type: 'change_input',
                value: e.target.value
            }
            dispatch(action)
        },
        clickButton() {
            let action = {
                type: 'add_item'
            }
            dispatch(action)
        }
    }
    class TodoList extends Component {
        constructor(props) {
            super(props)
        }
        render() {
            return (
                <div>
                    <div>
                        <input value={this.props.inputValue} onChange={this.props.inputChange} />
                        <button onClick={this.props.clickButton}>提交</button>
                    </div>
                    <ul>
                        {
                            this.props.list.map((item, index) => {
                                return (<li key={index}>{item}</li>)
                            })
                        }
                    </ul>
                </div>
            );
        }
    }

}
export default connect(stateToProps, dispatchToProps)(TodoList);
````



`connect`的作用是把UI组件（无状态组件）和业务逻辑代码的分开，然后通过connect再链接到一起，让代码更加清晰和易于维护。这也是`React-Redux`最大的特点。