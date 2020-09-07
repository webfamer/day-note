## react-hook

例子：

![mark](http://images.91miandan.top/blog/20200828/tPxdldvRiAHc.jpg?imageslim)

````react
import React ,{useState} from 'react';
function Example(){
  const [count,setCount] = useState(0); //0是状态变量count的初始值
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={()=>{setCount(count+1)}}>click me</button>
    </div> 
  )
}
export default Example;
````

### useState

用来声明状态变量

````react
const [count,setCount] = useState(0)
````

上面是数组解构的形式，第一个是状态变量 count，第二个是改变状态变量的方法 setCount

注意：useState**不能**在 if...else...这样的条件语句中调用，必须按相同的顺序进行渲染



### useEffect

跟react生命周期函数一致(``componentDidMount,componentDidUpdate``)

注意点：

1. React首次渲染和之后每次渲染都会调用一遍useEffect,而两个生命周期函数``componentDidMount``，``componentDidUpdate``分别表示首次渲染和更新导致的重新渲染
2. useEffect中定义的函数不会阻碍浏览器更新视图，也就是说这些函数是异步执行的，而``componentDidMount``，``componentDidUpdate``是同步执行的

 

``componentWillUnmount``在组件从 DOM 中移除之前立刻被调用。

``useEffect`` 可以实现一样的效果，具体如下

````react
function Index() {
  useEffect(() => {
    console.log('useEffect=>Index页面')
    return ()=>{
      console.log('溜了溜了，index页面')
    }
  },[])
  return <h2>JSPang.com</h2>;
}
````

useEffect中  用return一个函数的形式进行解绑，当useEffect的第二参数传空数组**[ ]**时，就是当组件将被销毁时才进行解绑，这也就实现了``componentWillUnmount``生命周期

useEffect的第二参数传入变量的话，就是只有当变量状态发生变化才会执行解绑函数

````react
useEffect(() => {
    console.log(`useEffect =>you clicked ${count} times`)
    return () => {
      console.log('=============')
    }
  }, [count])
````

### useContext

作用：跨越组件层级之间传递变量，实现共享

````react
import React,{useState,createContext,useContext} from 'react'

const countContext = createContext();
function Example4(){
    const [count,setCount] = useState(0);
    return(
        <div>
            <p>You clicked {count} times</p>
            <button onClick={()=>{setCount(count+1)}}>click me</button>

            <countContext.Provider value={count}> //里面的组件能够使用useContext传递的共享值
                <Counter/>
            </countContext.Provider>
        </div>
    )
}
function Counter(){
    const count = useContext(countContext);
    return (
        <div>
            {count}
        </div>
    )
}
export default Example4;
````

接收一个 context 对象（React.createContext 的返回值）并返回该 context 的当前值。当前的 context 值由上层组件中距离当前组件最近的 <MyContext.Provider> 的 value prop 决定。

[useContext参数详解](https://www.jianshu.com/p/b15e0c92d7c4)

### useReducer

redux中的reducer其实就是一个函数，这个函数接收两个参数，一个是状态，一个是用来控制业务逻辑的判断参数，看例子：

```javascript
function countReducer(state, action) {
    switch(action.type) {
        case 'add':
            return state + 1;
        case 'sub':
            return state - 1;
        default: 
            return state;
    }
}
```

useReducer可以增强Reducer,实现类似Redux的功能

[useReducer](https://links.jianshu.com/go?to=https%3A%2F%2Freactjs.org%2Fdocs%2Fhooks-reference.html%23usereducer)接收两个参数：

第一个参数：reducer函数。第二个参数：初始化的state。返回值为最新的state和dispatch函数（用来触发reducer函数，计算对应的state）。

简单小案例

````react

import React,{useReducer} from 'react';

function ReducerDemo(){
    const [count,dispatch] = useReducer((state,action)=>{
        switch(action){
            case 'add':
            return state+1
            case 'sub':
            return state -1
            default:
            return state
        }
    },0)
    return (
        <div>
            <h2>现在的分数是{count}</h2>
            <button onClick={()=>dispatch('add')}>Increment</button>
            <button onClick={()=>dispatch('sub')}>Decrement</button>
        </div>
    )
}

export default ReducerDemo;
````



**使用`useContext`和`useReducer`是可以实现类似`Redux`的效果**

[例子如下](https://juejin.im/post/5ceb37c851882520724c7504)

### useMemo

主要用来解决React hooks产生的无用渲染的性能问题

**useMemo()参数说明**

用过useEffect()大概会知道useEffect()的第二个参数的作用。useMemo()和useEffect()是一样的。

useEffect()第一个参数是执行函数，那......第二个参数：

- 若第二个参数为空，则每次渲染组件该段逻辑都会被执行，就不会根据传入的属性值来判断逻辑是否重新执行，这样写useMemo()也就毫无意义。
- 若第二个参数为空数组，则只会在渲染组件时执行一次，传入的属性值的更新也不会有作用。
- 所以useMemo()的第二个参数，数组中需要传入依赖的参数。

[useMemo解释](https://juejin.im/post/6844903925871722510)

例子:

父组件：

```react
function App() {
  const [name, setName] = useState('名称')
  const [content,setContent] = useState('内容')
  return (
      <>
        <button onClick={() => setName(new Date().getTime())}>name</button>
        <button onClick={() => setContent(new Date().getTime())}>content</button>
        <Button name={name}>{content}</Button>
      </>
  )
}
```

优化后的子组件：

````react
function Button({ name, children }) {
  function changeName(name) {
    console.log('11')
    return name + '改变name的方法'
  }

const otherName =  useMemo(()=>changeName(name),[name])
  return (
      <>
        <div>{otherName}</div>
        <div>{children}</div>
      </>

  )
}

export default Button
````

[案例](https://segmentfault.com/a/1190000018697490)

### useRef

````react
import React,{useRef} from 'react'
function Example8(){
    const inputEl = useRef(null);
    const onButtonClick =()=>{
        inputEl.current.value="Hello,JsPang"
        console.log(inputEl) //输出获取到的DOM节点
    }
    return(
        <>
        {/* 保存input的ref到inputEl */}
        <input type="text" ref={inputEl}/>
        <button onClick={onButtonClick}>在input上展示文字</button>
        </>
    )
}
export default Example8
````

