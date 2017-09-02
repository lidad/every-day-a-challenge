## 事件处理器

编写一个简单的自定义事件处理器   

要求：
1. 具备 on 方法绑定事件
2. 具备 off 方法解绑事件

测试用例如下：   

```
 var emitter = new EventEmitter();

 emitter.on('foo', function(e){
   console.log('listening foo event', e);
 });

 emitter.trigger('foo', {name : 'John'});
 emitter.off('foo');
 emitter.trigger('foo', {name : 'John'});
 
 function handler(args) {
   console.log('listening bar event');
 }
 
 emitter.on('bar', handler);
 emitter.on('bar', function() {
   console.log('other listening bar event');
 });
 emitter.trigger('bar', 'something');
 emitter.off('bar', handler);
 
 emitter.on('*', function(e){
   console.log('listening all events');
 });
 
 emitter.off('*');
```   

### 走起来！   

这里采用在对象内部维护一个事件队列的思路  

以下为具体实现  


（这里对```*```绑定的处理有点小问题，一旦绑定了```*```，之后为其他事件绑定回调的时候无法在绑定到```*```上，算是一个纰漏吧 ）

```
function EventEmitter() {
  this.eventList = [];
}


EventEmitter.prototype.on = function(eventName, callback) {
  if (!callback) {
    throw new Error('can not bind an event without a callback!')
  }

  const eventIndex = this.eventList.findIndex(event => event.eventName === eventName);
  const eventList = ~eventIndex ? this.eventList[eventIndex].eventList : [];
  if (eventName === '*' && !eventList.length) {
    this.eventList.reduce((tempEventList, eventObject) => {
      tempEventList.push(...eventObject.eventList);
      return tempEventList;
    }, eventList);
  }

  if (~eventList.indexOf(callback)) return;
  eventList.push(callback);
  this.eventList[~eventIndex ? eventIndex : this.eventList.length] = {
    eventName,
    eventList
  }
}


EventEmitter.prototype.off = function(eventName, callback) {
  const eventIndex = this.eventList.findIndex(event => event.eventName === eventName);
  if (!~eventIndex) return;
  if (!callback) {
    this._removeEvent(eventIndex);
  } else {
    const eventObject = this.eventList[eventIndex];
    const eventList = eventObject.eventList.reduce((tempEventList, cb) => tempEventList.concat(cb === callback ? [] : cb), []);
    eventList.length ? (this.eventList[eventIndex].eventList = eventList) : this._removeEvent(eventIndex)
  }
}

EventEmitter.prototype.trigger = function(eventName, cbArgs) {
  const eventObject = this.eventList.find(event => event.eventName === eventName);
  eventObject && eventObject.eventList.map(cb => void cb(cbArgs));
}

EventEmitter.prototype._removeEvent = function(index) {
  this.eventList.splice(index, 1)
}
```   

构造器的调用只是在其内部初始化了一个事件队列（说是队列不太严谨，队列严格的定义是先进先出的！）  

直接来看原型链上添加的方法   

```
EventEmitter.prototype.on = function(eventName, callback) {
  if (!callback) {
    throw new Error('can not bind an event without a callback!')
  }

  const eventIndex = this.eventList.findIndex(event => event.eventName === eventName);
  const eventList = ~eventIndex ? this.eventList[eventIndex].eventList : [];
  if (eventName === '*' && !eventList.length) {
    this.eventList.reduce((tempEventList, eventObject) => {
      tempEventList.push(...eventObject.eventList);
      return tempEventList;
    }, eventList);
  }

  if (~eventList.indexOf(callback)) return;
  eventList.push(callback);
  this.eventList[~eventIndex ? eventIndex : this.eventList.length] = {
    eventName,
    eventList
  }
}
```

事件队列中维护的数据结构为
```
{
  事件名<string>,
  事件对应的回调函数数组<Array<function>>
}
```
这样的一个结构   

```on()```方法中，首先判断是否有事件回调   

有事件回调的前提下，在事件队列中找到事件名对应的回调函数列表。如果已经绑定过一个声明的回调函数，那么无需操作，否则将该回调函数添加至事件的回调函数列表中  

注意一下这里对```*```的处理。如果绑定的事件名是“```*```”且没有为其绑定过回调，那么遍历其他事件的回调函数并将其添加进```*```的回调函数列表   

对于绑定事件的处理，除了要思考好数据结构的设计外基本上也没什么难度了   

```
EventEmitter.prototype.off = function(eventName, callback) {
  const eventIndex = this.eventList.findIndex(event => event.eventName === eventName);
  if (!~eventIndex) return;
  if (!callback) {
    this._removeEvent(eventIndex);
  } else {
    const eventObject = this.eventList[eventIndex];
    const eventList = eventObject.eventList.reduce((tempEventList, cb) => tempEventList.concat(cb === callback ? [] : cb), []);
    eventList.length ? (this.eventList[eventIndex].eventList = eventList) : this._removeEvent(eventIndex)
  }
}
```
```off()```先要判断为事件绑定过回调，没有则什么也不做   

另外```off```有对于第二个参数传入的不同有不一样的处理方式   

若是有第二个参数（回调函数），则在事件的回调函数列表里查找该回调并移除   

若是没有第二个参数，表示要移除整个事件，则在对象维护的事件列表中查找该事件并移除   

```
EventEmitter.prototype.trigger = function(eventName, cbArgs) {
  const eventObject = this.eventList.find(event => event.eventName === eventName);
  eventObject && eventObject.eventList.map(cb => void cb(cbArgs));
}
```

```trigger```只是执行事件注册的相应回调，其接受两个参数，第一个参数为时间名，第二个参数为注册回调的参数   

它在时间队列中查找相应的回掉函数列表并依次执行回调   

```
EventEmitter.prototype._removeEvent = function(index) {
  this.eventList.splice(index, 1)
}
```

```_removeEvent()```是一个私有的方法，用于在事件列表中将指定时间一次性移除