## 小题一道
先来看一下题目：   
```
f = function() {
  return true;
};
g = function() {
  return false;
};
(function() {
  if (g() && [] == ![]) {
    f = function f() {
      return false;
    };
  }
  function g() {
    return true;
  }
})();
f() // true or false ?
```

思考一下，再到浏览器的console里试一试，看看和你的结果是否一致   

### 走起来！

先说一下，最后f()运行的结果是false

我们将代码捋一遍，看看引擎在运行这段代码的时候做了哪些事情  
  
---
首先，在最开始定义了两个变量f与g

```
f = function() {
  return true;
};
g = function() {
  return false;
};
```
两个变量被定义为为函数表达式   
 
接着一个立即执行函数
```
(function() {
  if (g() && [] == ![]) {
    f = function f() {
      return false;
    };
  }
  function g() {
    return true;
  }
})();

```
注意，这里坑就来了！！

来看这个立即执行函数里的if语句
```
if (g() && [] == ![]) {
//...
}
```
稍有常识的人都可以看出，这个螳臂当车的```g()```，**并不是我们在前面声明的那个函数表达式**

在```if()```语句的下面，紧接着声明了一个函数g
```
function g() {
  return true;
}
```
**引擎在执行这段立即执行函数的时候，会将这个函数生命“提前”**
形如这样
```
(function() {
  function g() {
    return true;
  }
  if (g() && [] == ![]) {
    f = function f() {
      return false;
    };
  }
})();
```
也就是说if中的那个```g()```实际上结果是true

那么```g()```后面的那个```[]==![]```呢？

它的结果也是true（在考察作用域的题目里突然出现了个考察类型转换的也是有点乱入...）

这里涉及到了```==```与```!```运算时的类型转换
