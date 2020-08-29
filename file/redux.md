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

