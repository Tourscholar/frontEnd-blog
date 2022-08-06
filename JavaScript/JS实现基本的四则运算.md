### JS不用➕, ➖, ✖️, ➗实现基本的四则运算

#### 位运算
通过位运算即可实现加法操作
例如:
```
// 通过异或运算实现加法
1+0=1 1^0=1

0+1=1 0^1=1

0+0=0 0^0=0

// 通过按位与来判断是否进位
// 进位即左移一位
1 + 0 = 1 1&0 = 0

0 + 1 = 1 0&1 = 0

0 + 0 = 0 0&0 = 0

1 + 1 = 10 1&1 = 1
```
#### 加法
```javascript
const getSum = (a, b) => {
	return b === 0 ? a : getSum(a ^ b, (a & b) << 1)
}
```
#### 减法
减法可以看出加法加上一个正数取反: a -b = a + (-b) 
由于正数的补码等于自身, 负数的补码等于按位取反加一, 所以a - b = a + (-b) = a + (~b + 1)
```javascript
const getSub = (a, b) => {
	return getSum(a, getSum(~b, 1))
}
```
#### 乘法
利用二进制移位
当有两个数a, b相乘时, 将a除以2, b乘以2, 当a位奇数时累加b的值即可得出a * b的值
```javascript
const getMultiply = (a, b) => {
	let sum = 0;
	while (a) {
		// 将a与1做与运算, 如果结果为1, 则a为奇数
		if (a & 1 == 1) {
			sum += b;
			a >>= 1;
			b <<= 1;
		}
	}
	return sum;
}
```
#### 除法
与乘法思路相似, 将被除数每次右移一位, 同时检查除数是否大于被除数, 如果不是则将将除数的次数左移一位, 直到除数乘以除数的次数大于被除数为止
```javascript
const getDiv = (a, b) => {
	let mulDis = a, res = 0;
	while (mulDis >= b) {
		let mulTimes = 1;
		while (b * mulTimes <= (mulDis >> 1)) {
			mulTimes <<= 1;
		}
		res += mulTimes;
		mulDis -= b * mulTimes;
	}
	return res;
}
