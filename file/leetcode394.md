# 题目

## 题目描述

```
给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: k[encoded_string]，表示其中方括号内部的 encoded_string 正好重复 k 次。注意 k 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 k ，例如不会出现像 3a 或 2[4] 的输入。

示例:

s = "3[a]2[bc]", 返回 "aaabcbc".
s = "3[a2[c]]", 返回 "accaccacc".
s = "2[abc]3[cd]ef", 返回 "abcabccdcdcdef".

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/decode-string
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```


# 我的回答

## 思路

从头到尾遍历字符串，碰到数字，开始拼接，等碰到'['的时候，把前面拼接的数字放入建立的栈中，然后跳过本次循环（防止把‘['也放入栈），碰到字符也是直接放入栈，碰到‘]’的时候，开始出栈，把栈内的字符进行重复处理之后放回栈中，最后就构建了一个存储着拼接字符的栈

## 图解

![mark](http://images.91miandan.top/blog/20200901/CTTaQlJVXgvp.png?imageslim)

## 代码

JavaScript Code
```js
      var decodeString = function (s) {
        let i =0;
        let digit = '';
        let stack =[];
        while(s[i]){
          if(s[i] =='['){
            stack.push(digit);
            digit ='';
            i++;
            continue;
          }else if(/\d+/.test(s[i])){
            digit+=s[i];
          }else if(s[i] ==']'){ //碰到']'符号开始对stack进行解密
              let arrItem = stack.pop();
              let str = ''
             while(!/\d+/.test(arrItem)){ //如果不是数字，就对字符进行叠加
                str = arrItem+str;//栈内的数据是按数字，字符这样排列
                arrItem = stack.pop();//，所以出栈不是数字的时候，就是第一次处理完毕了
             }
             stack.push(str.repeat(arrItem))
          }else{
            stack.push(s[i])
          }
          i++
        }
        console.log(stack)
      };

```

Python Code
```py

```


# 参考回答