### 今天我们来一发懒逼，学名lazyMan

什么是懒逼，懒逼是这样：

```
Lazyman('Sam').eat('shit').sleep(10).eat('shit');
// Hi, I'm Sam, I'm so lazy...
// eat shit..
//...(等待10秒)
// wake me up after 10 secends...
// eat shit..
```

#### 对于懒逼，有以下几点：
- 懒逼的行为可以链式调用，所以在调用后必须返回实例
- 对象的内部要维护一个懒逼的行为队列，每次懒逼要干什么就出队列执行
