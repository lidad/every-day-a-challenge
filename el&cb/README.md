## event loop 和回调的小题
今天来个简单的小题，旨在考察引擎的event loop与回调的处理   

来看一下题目：
```
console.log(1);
setTimeout(() => {
  console.log(2)
}, 0);
new Promise((resolve, reject) => {
  resolve();
  console.log(3);
}).then(() => {
  console.log(4);
});
console.log(5);
//数字的输出顺序是什么？
```
题目很简单，如果对引擎的event loop有所了解的话，你应该立刻就能给出正确答案

### 走起来！

先说一下，上面的打印顺序是
```
1
3
5
4
2
```

我们来把代码捋一遍，看看引擎在执行的时候做了什么。 
  
首先打印了1，没什么疑问
```
console.log(1);
```
接着是一个时间为0的计时器，在0秒后打印2
```
setTimeout(() => {
  console.log(2)
}, 0);
```
计时器后是一个```promise```，它立刻决议并打印了3
```
new Promise((resolve, reject) => {
  resolve();
  console.log(3);
})
```
之后```then()```的fulfilled中打印了5
```
then(() => {
  console.log(4);
})
```
最后打印了5
```
console.log(5);
```

那么，为什么会出现以上那样的结果呢？这就涉及到了引擎的event loop

### event loop

关于引擎的event loop，我之前有写过一篇[文章](http://www.dadel.live/2017/05/26/js%E5%BC%95%E6%93%8E%E7%9A%84%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF%E7%9A%84%E4%B8%80%E4%BA%9B%E9%A2%98%E5%A4%96%E8%AF%9D/)，感兴趣的话可以去瞅瞅   
这里就V8的event loop简单说明一下
