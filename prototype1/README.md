## 考察原型链的一道小题
今天的这道小题简单的考察了一下大家对原型链的理解，来看看题目：

```
var A = {
  n: 1
}
var B = function() {
  this.n = 9999
};
var C = function() {
  var n = 8888
};

B.prototype = A;
C.prototype = A;

var b = new B();
var c = new C();

A.n++;

console.log(b.n);
console.log(c.n);
```   

要注意```C```定义中的那个```n```的声明哈~   
