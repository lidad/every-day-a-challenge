## 小题一道
来先看一下题目：   
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

我们将代码捋一遍，引擎在运行这段代码的时候做了哪些事情   

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

