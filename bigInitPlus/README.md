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