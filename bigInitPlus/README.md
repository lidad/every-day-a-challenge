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