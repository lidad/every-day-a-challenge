## 一道考察作用域的小题

这道题依然考察了作用域与变量声明，角度比较奇特，来看看

```
var name = 'World!';
(function() {
  if (typeof name === 'undefined') {
    var name = 'Jack';
    console.log('Goodbye ' + name);
  } else {
    console.log('Hello ' + name);
  }
})();

(function() {
  if (typeof name === 'undefined') {
    name = 'Jack';
    console.log('Goodbye ' + name);
  } else {
    console.log('Hello ' + name);
  }
})();

(function() {
  if (typeof name === 'undefined') {
    console.log('Goodbye ' + name);
  } else {
    var name = 'Jack';
    console.log('Hello ' + name);
  }
})();

```

注意看两个立即执行函数里对```name```的声明

### 走起来!

先说一下最后的输出：
```
Goodbye Jack
Hello World
Goodbye undefined
```

只要你知道变量声明的原理，这道题肯定so easy