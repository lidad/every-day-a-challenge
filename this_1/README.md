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