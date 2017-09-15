## 如何实现一个深拷贝
深拷贝算得上是一个相对具有深度的题目了   

一般在面试的过程中我也比较喜欢去问面试者深拷贝相关的东西   

如果一个面试者对深拷贝能有一些自己的理解和实现，我会觉得他对JavaScript的学习态度有些深度和广度了，是一个不错的印象  

### 为何要深拷贝？   

做题前先扯一些没用的，为什么我们会需要深拷贝这个东西呢？   

要知道的是JavaScript中对象调用的都是其引用    

看一下下面的代码   

```
const a = [1,2,3];
b = a;
b.push(4);
//此时的a？

const c = {
  d: 1
}
c.e = 2
//c声明为常量怎么还可以修改？

const f = c;
f.d = 3;
//此时c.d？
```   

由于对象引用调用的关系，在为数组```b```添加了元素```4```之后，数组```a```也受到了影响变成了```[1,2,3,4]```   

而```c```仅仅保存的是对对象的引用，为对象添加或修改属性，并没有改变其引用，```c```虽然被声明为常量但不会有问题    

JavaScript的这种机制是一种双刃剑，对于对象的处理一定程度上节省了内存空间，在引用关系的基础上一些场景下会很方便我们的操作。但如果对其不是很熟悉，就会埋下一些暗坑，有时候代码读过去觉得是好的，跑起来就会出错   

而对于react类库中的pure render来说，官方为我们提供了一个基于浅比较的[pureComponent](https://facebook.github.io/react/docs/react-api.html)，但由于是浅比较，在涉及复杂数据结构深层变化的时候它就GG了。。深拷贝可以解决这个问题，无奈其本身也有性能问题，对于追求高性能的react这也是相悖的（immutable快出来，哈哈）   

### 体验一下深拷贝

JavaScript已经为我们提供了一些具有深拷贝性质的API   

比如说想要深拷贝一个数组，```concat()```与```slice()```都可以实现   

```
var a = [1,2,3];
var fakeA = a;
var deepA = a.slice();
var anotherDeepA= a.concat();

deepA  //[1,2,3]
anotherDeepA  //[1,2,3]

deepA === a  //false
anotherDeepA === a  //false
fakeA === a  //true

deepA.push('deepA');
anotherDeepA.push('otherDeepA');
fakeA.push('a');

a  //[1,2,3,'a'] 
fakeA  //[1,2,3,'a'] 
deepA  //[1,2,3,'deepA']
anotherDeepA  //[1,2,3,'otherDeepA']
```   

可以看到，经过深拷贝的```deepA```与```anotherDeepA```已经与原来的数组```a```完全脱离了联系，不再会相互影响   

深拷贝复制了一个对象，但和之前的对象具有不同的引用。经过深拷贝的对象与原对象相比具有相同的值，相较于浅拷贝其在内存中保存了两份   

要实现深拷贝其实也就是要实现两件事：   

1. 返回一个新的引用
2. 新的引用中保存了和原来对象相同的内容   

   
这里给出两种方法，一种基于json的序列化与反序列化，一种基于递归

#### json的序列化与反序列化

代码很简单：   

```
function deepClone(initalObj){
  return JSON.parse(JSON.stringify(initalObj));
}
```
