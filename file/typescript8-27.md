## 断言

instanceof 用于判断一个变量是否是某个对象的实例

````javascript
//例一
var a = new Array();

alert(a instanceof Array);//true
同时 alert(a instanceof Object) //也会返回true

因为Array 是object的子类

//例二
function test(){};
var a = new test();
alert(a instanceof test) 会返回true
````



**兼容**

````typescript
interface Animal{
    name:string;
}
interface Cat{
    name:string;
    run():void;
}
let tom:Cat ={
    name:'Tom',
    run:()=>{
        console.log('run')
    }
}
let animal: Animal = tom;
````

上面例子中，``Cat``包含了``Animal``中的所有属性，除此之外，它还有一个额外的方法run,TypeScript 并不关心 `Cat` 和 `Animal` 之间定义时是什么关系，而只会看它们最终的结构有什么关系——所以它与 `Cat extends Animal` 是等价的

````typescript
interface Animal {
    name: string;
}
interface Cat extends Animal {
    run(): void;
}
````

我们把它换成 TypeScript 中更专业的说法，即：`Animal` 兼容 `Cat`