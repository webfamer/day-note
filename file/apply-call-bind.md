## apply call bind的区别

三者都是改变this的指向，具体区别如下

1. 需要传递多个参数的时候，apply传递的是数组，call传递的是多个参数

2. bind返回的是一个新的函数，需要调用才会被执行

   ````javascript
   var name = '小王',age = 17;
   var obj = {
   	name:'小张',
   	objAge：this.age,
   	myFun:function(fm,t){
   	console.log(this.name+"年龄"+this.age,"来自"+fm+"去往"+t);
   	}
   }
   var db = {
   	name:'德玛',
   	age:99
   }
   
   
   
   //输出结果
   obj.myFun.call(db,'成都','上海')；　　　　 // 德玛 年龄 99  来自 成都去往上海
   obj.myFun.apply(db,['成都','上海']);      // 德玛 年龄 99  来自 成都去往上海  
   obj.myFun.bind(db,'成都','上海')();       // 德玛 年龄 99  来自 成都去往上海
   obj.myFun.bind(db,['成都','上海'])();　　 // 德玛 年龄 99  来自 成都, 上海去往 undefined
   ````

   ​	[参考链接](https://www.runoob.com/w3cnote/js-call-apply-bind.html)

## argument参数

在函数体内可以通过arguments 对象来访问参数数组，从而获取传递给函数的每一个参数。

[参考链接](https://www.cnblogs.com/greenteaone/p/9225337.html)