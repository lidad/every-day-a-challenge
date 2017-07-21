## 今天来实现一个bind()方法

看一下bind()的定义：
>bind() 函数会创建一个新函数（称为绑定函数），新函数与被调函数（绑定函数的目标函数）具有相同的函数体（在 ECMAScript 5 规范中内置的call属性）。当目标函数被调用时 this 值绑定到 bind() 的第一个参数，该参数不能被重写。绑定函数被调用时，bind() 也接受预设的参数提供给原函数。一个绑定函数也能使用new操作符创建对象：这种行为就像把原函数当成构造器。提供的 this 值被忽略，同时调用时的参数被提供给模拟函数。

bind()可以实现三个功能
- 改变调用函数的this
- 函数柯里化（预设参数给原函数）
- 使用new时将原函数当做构造器并忽略提供的this值

## 走起来！

这里参照MDN上的polyfill来给出一种实现   
（此实现基于apply()，关于apply()的实现可以参照[这里](https://github.com/lidad/every-day-a-challenge/tree/master/apply)）

```
Function.prototype.bind || Object.defineProperty(Function.prototype, 'bind', {
  value: function(context, ...curryArgs) {
    const fn = this;
    const noop = function() {};
    const bound = function(...args) {
      return fn.apply(context, [
        ...curryArgs,
        ...args
      ]);
    }

    noop.prototype = this.prototype;
    bound.prototype = new noop();

    return bound;
  },
  enumerable: false
})
```
