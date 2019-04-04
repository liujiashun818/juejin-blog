### 一、原型链 ###
原型链的prototype和__proto__的区别；
prototype 在 new 示例后会被转为 __proto__

__proto__是非标准化的；
所有东西的原型链向上延伸到原型链的顶端，是什么；
Object.prototype.__proto__，结果是null.


```
Function本身就是函数，Function.__proto__是标准的内置对象Function.prototype，
Function.prototype.__proto__是标准的内置对象Object.prototype
```
