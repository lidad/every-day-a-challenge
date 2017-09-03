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

```throttle()```方法内部定义了上次执行时间点与```setTimeout()```所返回的时间数据   

而要保存上次函数执行的时间点主要是为了阈值的作用   

#### 阈值

如果一个函数总是被节流，其实并不是一个很好的用户体验   

譬如在resize页面时，总是节流，页面没有相应，反而显得有些奇怪了   

这时为```throttle()```设置一个阈值，若是被节流的函数再次响应时等于或超过了阈值的时间，那么就让它执行一次，来放宽节流的限制