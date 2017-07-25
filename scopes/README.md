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
    function g() {
      return true;
    }
  }
})();
f() // true or false ?
```

思考一下，再到浏览器的console里试一试，看看和你的结果是否一致
