## 函数节流（throttle）

简单实现以下函数节流，用例如下：

```
function throttle() {
//code here..
}

window.addEventListener('scroll', throttle(func, 50), false);
```

### 走起来！   

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