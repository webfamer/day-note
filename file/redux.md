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

