# 题目

## 题目描述

```
设计一个支持在平均 时间复杂度 O(1) 下，执行以下操作的数据结构。

insert(val)：当元素 val 不存在时，向集合中插入该项。
remove(val)：元素 val 存在时，从集合中移除该项。
getRandom：随机返回现有集合中的一项。每个元素应该有相同的概率被返回。
示例 :

// 初始化一个空的集合。
RandomizedSet randomSet = new RandomizedSet();

// 向集合中插入 1 。返回 true 表示 1 被成功地插入。
randomSet.insert(1);

// 返回 false ，表示集合中不存在 2 。
randomSet.remove(2);

// 向集合中插入 2 。返回 true 。集合现在包含 [1,2] 。
randomSet.insert(2);

// getRandom 应随机返回 1 或 2 。
randomSet.getRandom();

// 从集合中移除 1 ，返回 true 。集合现在包含 [2] 。
randomSet.remove(1);

// 2 已在集合中，所以返回 false 。
randomSet.insert(2);

// 由于 2 是集合中唯一的数字，getRandom 总是返回 2 。
randomSet.getRandom();

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/insert-delete-getrandom-o1
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```


# 我的回答

## 思路

一个数组用来存储数据，另外一个哈希表用来记录，key就是存入的值，value就是在数组中的位置。

删除的时候，就直接把数字最后一个值赋予给要交换的值，再把数组最后一个值进行删除，再把哈希表内对应的数据进行更新，这样删除的复杂度也是O(1)了；

## 图解

![mark](http://images.91miandan.top/blog/20200910/yAuO3CyH4wMb.jpg?imageslim)

## 代码

JavaScript Code
```js
 var RandomizedSet = function () {
        this.arr = [];
        this.mapSet = {};
      };
      RandomizedSet.prototype.insert = function (val) {
        if (this.mapSet[val]) {
          return false;
        } else {
          this.arr.push(val);
          this.mapSet[val] = this.arr.length - 1;
          return true;
        }
      };
      RandomizedSet.prototype.remove = function (val) {
          debugger;
        if (val in this.mapSet) {
          let index = this.mapSet[val];
          let lastVal = this.arr[this.arr.length - 1];
          this.arr[index] = lastVal;
          this.arr.pop();
          this.mapSet[lastVal] = index;
          delete this.mapSet[val];
          return true;
        } else {
          return false;
        }
      };
      RandomizedSet.prototype.getRandom = function () {
        let index = Math.floor(Math.random(0, 1) * this.arr.length);
        return this.arr[index];
      };
```

Python Code
```py

```


# 参考回答

[参考回答](https://leetcode-cn.com/problems/insert-delete-getrandom-o1/solution/guan-fang-jie-fa-javascript-ban-ben-by-jack-108/)