### 今天我们来一发懒逼，学名lazyMan

什么是懒逼，懒逼是这样：

```
Lazyman('Sam').eat('shit').sleep(10).eat('shit');
// Hi, I'm Sam, I'm so lazy...
// eating shit..
//...(等待10秒)
// wake me up after 10 seconds...
// eating shit..

Lazyman('Sam').eat('shit').sleepFirst(10).eat('shit');
//...(等待10秒)
// aha, sleep for 10 seconds
// Hi, I'm Sam, I'm so lazy...
// eating shit..
// eating shit..

```

#### 对于懒逼，有以下几点：
- 懒逼的行为可以链式调用，所以在调用后必须返回实例
- 对象的内部要维护一个懒逼的行为队列，每次懒逼要干什么就出队列执行

#### 基本版

基本版的懒逼基于es5实现，网上的代码一大坨，这里给出一个实现：


```
function _LazyMan(name) {
  this.tasks = [];
  //设置默认值防止输入错误
  this.defaultOptions = {
    name: 'lazy man',
    time: 10,
    food: 'nothing'
  };
  var _this = this;
  var lName = name || _this.defaultOptions.name;
  this.tasks.push(function() {
    console.log("Hi, I'm " + lName + ", I'm so lazy...");
    _this.next();
  })

  setTimeout(function() {
    _this.next();
  }, 0);
}

_LazyMan.prototype.next = function() {
  var task = this.tasks.shift();
  task && task();
}
```

为lazy man添加行为

```

_LazyMan.prototype.eat = function(food) {
  var _this = this;
  var lFood = food || _this.defaultOptions.food;
  _this.tasks.push(function() {
    console.log("eating " + lFood + "...");
    _this.next();
  })
  return this;
}

_LazyMan.prototype.sleep = function(time) {
  var _this = this;
  var lTime = time || _this.defaultOptions.time;
  _this.tasks.push(function() {
    setTimeout(function() {
      console.log("wake me up after " + lTime + " seconds...");
      _this.next();
    }, lTime * 1000);
  })
  return this;
}

_LazyMan.prototype.sleepFirst = function(time) {
  var _this = this;
  var lTime = time || _this.defaultOptions.time;
  _this.tasks.unshift(function() {//注意此处是将事件从列表前部插进去的
    setTimeout(function() {
      console.log("aha, sleep for " + lTime + " seconds...");
      _this.next();
    }, lTime * 1000);
  })
  return this;
}
```
返回构造函数
```
var LazyMan = function(name) {
  return new _LazyMan(name);
}

```

#### 进阶版

进阶版主要由es6编写

- 使用es6 class
- next方法换做async await来处理
- lazy man的行为使用promise
- promise中的决议使用do表达式

写出构造函数
```
class _LazyMan {
  constructor(name) {
    this.tasks = [];
    this.defaultOptions = {
      name: 'lazy man',
      time: 10,
      food: 'nothing'
    };

    this.tasks.push(() => {
      console.log(`Hi, I'm ${name || this.defaultOptions.name}, I'm so lazy...`);
      this.next();
    });

    setTimeout(() => this.next(), 0);
  }

  async next() {
    const task = this.tasks.shift();
    task && await task();
  }
}
```

lazy man的行为
```
eat(food = this.defaultOptions.food) {
  this.tasks.push(() => Promise.resolve(do {
    console.log(`eating ${food}...`);
    this.next();
  }))
  return this;
}

sleep(time = this.defaultOptions.time) {
  this.tasks.push(() => new Promise((resolve, reject) => {
    setTimeout(() => resolve(do {
      console.log(`wake me up after ${time} seconds...`);
      this.next();
    }), time * 1000);
  }))
  return this;
}

sleepFirst(time = this.defaultOptions.time) {
  this.tasks.unshift(() => new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(do {
        console.log(`aha, sleep for ${time} seconds...`);
        this.next();
      })
    }, time * 1000);
  }))
  return this;
}
```

返回构造函数的调用
```
const LazyMan = name => new _LazyMan(name);

```

#### 超级进阶版

超级进阶版使用了es7的decorator来为lazy man来添加行为
