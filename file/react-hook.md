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