

## typescript 声明文件

声明文件必须以 .d.ts为后缀

### 第三方文件声明

推荐使用``@types``统一管理第三方库的声明文件

以jQuery举例

````typescript
npm install @types/jquery --save-dev
````



可以在[这个页面](https://www.typescriptlang.org/dt/search)声明文件

**全局变量的声明文件主要有以下几种语法：**

- [`declare var`](https://ts.xcatliu.com/basics/declaration-files.html#declare-var) 声明全局变量
- [`declare function`](https://ts.xcatliu.com/basics/declaration-files.html#declare-function) 声明全局方法
- [`declare class`](https://ts.xcatliu.com/basics/declaration-files.html#declare-class) 声明全局类
- [`declare enum`](https://ts.xcatliu.com/basics/declaration-files.html#declare-enum) 声明全局枚举类型
- [`declare namespace`](https://ts.xcatliu.com/basics/declaration-files.html#declare-namespace) 声明（含有子属性的）全局对象
- [`interface` 和 `type`](https://ts.xcatliu.com/basics/declaration-files.html#interface-he-type) 声明全局类型

#### `declare var`

当声明用 ``declare var `` 和``declare let`` 没有什么区别 ，但是用``declare const ``时，表示是全局变量的常量，就不允许修改值了

注意：声明语句中只能定义类型，不能在声明语句中定义具体的实现：

````typescript
declare const jQuery = function(selector) {
    return document.querySelector(selector);
};
// ERROR: An implementation cannot be declared in ambient contexts.
````

#### `declare function`

在函数类型的声明语句中，函数重载也是支持的

```ts
// src/jQuery.d.ts

declare function jQuery(selector: string): any;
declare function jQuery(domReadyCallback: () => any): any;
```

````ts
// src/index.ts

jQuery('#foo');
jQuery(function() {
    alert('Dom Ready!');
});
````

#### `declare class`

`declare class` 语句也只能用来定义类型，不能用来定义具体的实现,不然会报错

```ts
// src/Animal.d.ts

declare class Animal {
    name: string;
    constructor(name: string);
    sayHi() {
        return `My name is ${this.name}`;
    };
    // ERROR: An implementation cannot be declared in ambient contexts.
}
```

#### `declare namespace`

`declare namespace` 还是比较常用的，它用来表示全局变量是一个对象，包含很多子属性。

比如 `jQuery` 是一个全局变量，它是一个对象，提供了一个 `jQuery.ajax` 方法可以调用，那么我们就应该使用 `declare namespace jQuery` 来声明这个拥有多个子属性的全局变量。

````ts
// src/jQuery.d.ts

declare namespace jQuery {
    function ajax(url: string, settings?: any): void;
}
````

````ts
// src/index.ts

jQuery.ajax('/api/get_something');
````

#### 声明合并

假如 jQuery 既是一个函数，可以直接被调用 `jQuery('#foo')`，又是一个对象，拥有子属性 `jQuery.ajax()`（事实确实如此），那么我们可以组合多个声明语句，它们会不冲突的合并起来[14](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/14-declaration-merging)：

````ts
// src/jQuery.d.ts

declare function jQuery(selector: string): any;
declare namespace jQuery {
    function ajax(url: string, settings?: any): void;
}
````

````ts
// src/index.ts

jQuery('#foo');
jQuery.ajax('/api/get_something');
````

#### `export default`

在 ES6 模块系统中，使用 `export default` 可以导出一个默认值，使用方可以用 `import foo from 'foo'` 而不是 `import { foo } from 'foo'` 来导入这个默认值。

注意，只有 `function`、`class` 和 `interface` 可以直接默认导出，其他的变量需要先定义出来，再默认导出

错误写法：

````typescript
// types/foo/index.d.ts

export default enum Directions {
// ERROR: Expression expected.
    Up,
    Down,
    Left,
    Right
}
````

正确写法：

````typescript
// types/foo/index.d.ts

declare enum Directions {
    Up,
    Down,
    Left,
    Right
}

export default Directions;
````

#### `export namespace`

与 `declare namespace` 类似，`export namespace` 用来导出一个拥有子属性的对象:

```ts
export namespace foo {
    const name: string;
    namespace bar {
        function baz(): string;
    }
}
```

```ts
// src/index.ts

import { foo } from 'foo';

console.log(foo.name);
foo.bar.baz();
```

#### `export =`

在 commonjs 规范中，我们用以下方式来导出一个模块：

```js
// 整体导出
module.exports = foo;
// 单个导出
exports.bar = bar;
```

在 ts 中，针对这种模块导出，有多种方式可以导入，第一种方式是 `const ... = require`：

```js
// 整体导入
const foo = require('foo');
// 单个导入
const bar = require('foo').bar;
```

````ts
// 整体导入
import * as foo from 'foo';
````

##### 拆分声明文件

```ts
// node_modules/@types/jquery/index.d.ts

/// <reference types="sizzle" />
/// <reference path="JQueryStatic.d.ts" />
/// <reference path="JQuery.d.ts" />
/// <reference path="misc.d.ts" />
/// <reference path="legacy.d.ts" />

export = jQuery;
```

其中用到了 `types` 和 `path` 两种不同的指令。它们的区别是：`types` 用于声明对另一个库的依赖，而 `path` 用于声明对另一个文件的依赖。