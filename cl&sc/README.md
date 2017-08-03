## 一道考察闭包与作用域的小题

这道题简单的考察了闭包与作用域，很有意思   

```
function test() {
  var n = 1;

  function add() {
    n++;
    console.log(n)
  }

  return {n: n, add: add}
}

var result = test();
var result2 = test();

result.add();
result.add();
console.log(result.n);
result2.add();
```

注意```test()```中定义和返回的东西，不要看走眼咯😀   

### 走起来！

先说一下，最后输出的结果是   

```
2
3
1
2
```
