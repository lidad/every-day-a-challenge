## 考察this的一道小题

这道题来源于SegmentFault的一片讲解this的[文章](http://mp.weixin.qq.com/s/haFVIlx-CBNtDCpA3BYr9g)，
他的考察点很有意思，搬过来搞一下   

```
function foo(arg) {
  this.a = arg;
  return a;
}

var a = foo(1);
var b = foo(10);

console.log(a.a);
console.log(b.a);
```   

如果你能说出正确答案，想必也会对其考察方式叹为观止吧，哈哈~   

### 走起来！

先说一下答案：
```
undefined
10
```
最大的疑惑肯定就是undefined的了

对于```a.a```，可能有的人会说是1，有的人会说是10   

细读一下这段代码   

首先，引擎在跑这段代码时会经历**变量声明的提升**，形如如下所示：
```
function foo(arg) {
  this.a = arg;
  return a;
}

var a;
var b;

a = foo(1);
b = foo(10);
```  

在```a```与```b```的赋值语句执行时，两个变量已经被声明过了   

也就是说```foo(1)```与```foo(10)```在执行时引擎中这段作用域里```a```与```b```都已经被声明过！