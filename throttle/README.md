## 函数节流（throttle）

简单实现以下函数节流，用例如下：

```
function throttle() {
//code here..
}

window.addEventListener('scroll', throttle(func, 50), false);
```
这个算是一道很经典的面试题了，和其他面试题不同，他很具有实用价值，来看看   

### 走起来！   

这里所采用的方案接受三个参数，分别是被节流的函数，节流时间与阈值   

关于阈值接下来会说明，先来看一下具体实现   

```
function throttle(func, duration, threshold) {
  let timer;
  let lastTime = 0;

  return () => {
    const nowTime = Date.now();
    !lastTime && (lastTime = nowTime);

    clearTimeout(timer);

    if (threshold && nowTime - lastTime > threshold) {
      lastTime = nowTime;
      func();
    } else {
      timer = setTimeout(function() {
        lastTime = 0;
        func();
      }, duration)
    }
  }
}
```   

由于函数节流要保存上一次函数执行时的时间点，这里采用了闭包的方法   

```throttle()```方法内部定义了上次执行时间点与```setTimeout()```所返回的定时器编号  

而要保存上次函数执行的时间点主要是为了阈值的作用   

#### 阈值

如果一个函数总是被节流，其实并不是一个很好的用户体验   

譬如在resize页面时，总是节流，页面没有了响应，反而显得有些奇怪了   

这时为```throttle()```设置一个阈值，若是被节流的函数再次响应时等于或超过了阈值的时间，那么就让它执行一次，来放宽节流的限制   

在resize时加入一个500ms的阈值，若是本次函数响应距离上次被执行有500ms，那么就让它执行一次   

代码很简单，稍微来过一下

```
let timer;//需要保存的计时器编号
let lastTime = 0;//上一次函数执行的时间点
```

这两个就是需要使用闭包来保留在“全局”的变量

```
return () => {
    const nowTime = Date.now();
    !lastTime && (lastTime = nowTime);

    //清空计时器编号
    clearTimeout(timer);

    //如果离上次执行时间大于阈值，那么执行一次
    if (threshold && nowTime - lastTime > threshold) {
      lastTime = nowTime;
      func();
    } else {
      //被节流
      timer = setTimeout(function() {
        //注意这里对上一次执行时间点的重置操作
        lastTime = 0;
        func();
      }, duration)
    }
  }
```

很通用的思路，若是函数被响应，延时```duration```让其执行   

由于每次都有对计时器清空与重新开始计时的操作，只要响应时间小于```duration```，函数就不会运行   

同时引入阈值放宽限制，只要离上次函数运行时间大于阈值，就执行一次函数