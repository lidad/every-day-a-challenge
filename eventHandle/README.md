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