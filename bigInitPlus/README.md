## 大整数加法
实现一个大整数的加法器   

大整数可能比Number.MAX_VALUE大，所以...好好思考一下哈，很简单的

```
var BigInt = function (str) {
 //code here
};

BigInt.prototype.plus = function(bigint) {
 //code here
};

BigInt.prototype.toString = function () {
 //code here
};

var bigint1 = new BigInt('1234232453525454546445458814343421545454545454');
var bigint2 = new BigInt('1234232453525454546445459914343421536654545454');

console.log(bigint1.plus(bigint2));
```

### 走起来！   

先来给出一个实现   

```
var BigInt = function(str) {
  this.bigInt = str;
};

BigInt.prototype.plus = function(bigint) {
  const bigInt = '' + bigint;
  const { longNumber, shortNumber } = this.bigInt.length >= bigInt.length ? {
    longNumber: this.bigInt.split(''),
    shortNumber: bigInt.split('')
  } : {
    longNumber: bigInt.split(''),
    shortNumber: this.bigInt.split('')
  }

  const { result } = longNumber.reduceRight((tempResult, firstSingleNum) => {
    const { result, carry } = tempResult;
    const secondSingleNum = shortNumber.pop() || 0;
    const tempAddResult = +firstSingleNum + (+secondSingleNum) + carry;

    return {
      result: tempAddResult % 10 + result,
      carry: tempAddResult > 9
    }
  }, {
    result: '',
    carry: 0
  });

  return result;
};

BigInt.prototype.toString = function() {
  return this.bigInt;
};
```   

这道题考察的点其实是js中```string```类型的长度

string类型长度很长，以前在知乎上贺老的答案中看到过具体数字，很惭愧没有深究。。。只是记住了贺老的一句话：“实际情况中```string```的长度只会受引擎内存的限制”。也就是说规范中它的长度已经超过了现如今引擎的内存。。。。。   

对于这道题来说，其关键在于以```string```为存储方式来重现整数的加法
