### JavaScript数组操作
```JavasScript```数组操作, 主要包括```Array```对象原型方法以及常用操作去重、扁平化、排序等

#### Array.prototype
##### forEach
```arr.forEach(callback(currentValue [, index [, array]])[, thisArg])```
  - ```callback```为数组中每个元素执行的函数, 该函数接收一至三个参数
  - ```currentValue```数组中正在处理等当前元素的索引
  - ```array```可选[表示正在操作的数组]
  - ```thisArg```可选[当执行回调函数时, 用作this的值, 当使用箭头函数时此参数将被忽略]

forEach()对数组的每一个元素执行一次给定的函数
```javascript
let arr = [1, 2, 3, 4, 5];
let obj = {a: 1};
arr.forEach(function(currentValue, index, array) {
  console.log("当前值：", currentValue);		// 1
  console.log("当前值索引：", index);				// 0
  console.log("当前处理数组：", array);		 // (5)[1, 2, 3, 4, 5]
  console.log("当前this指向：", this);			// {a: 1}
  console.log("结束一次回调, 无需返回值");
  console.log("");
}, obj);

console.log(arr);				// [1, 2, 3, 4, 5]不改变原数组
```

##### map
```arr.map(callback(currentValue [, index [, array]])[, thisArg])```
  - ```callback```为数组每个元素执行的函数, 该函数接收一至三个参数
  -  ```currentValue```数组中正在处理的当前元素
  - ```index```可选[数组中正在处理的当前元素的索引]
  - ```array```可选[正在操作的数组]
  -  ```thisArg```可选[当执行回调函数时, 用作this的值, 当使用箭头函数时此函数将被忽略]

map()创建一个新数组, 其结果是该数组中的每个元素都被调用一次提供的函数后的返回值
```javascript
let arr = [1, 2, 3, 4, 5];
let obj = {a: 1};
let newArr = arr.map(function(currentValue, index, array) {
  console.log("当前值：", currentValue);		// 1
  console.log("当前值索引：", index);				// 0
  console.log("当前处理数组：", array);		 // (5)[1, 2, 3, 4, 5]
  console.log("当前this指向: ", this);		 // {a: 1}
  console.log("");
  return crrentValue + 10;
}, obj);
console.log(newArr);		// [11, 12, 13, 14, 15]
console.log(arr);				// [1, 2, 3, 4, 5]不改变原数组
```

##### push
```arr.push(element1[,..., elementN])```
```elementN```被添加到数组末尾的元素
push()将一个或多个元素添加到数组的末尾, 并返回该数组的长度
```javascript
let arr = ['a', 'b', 'c', 'd', 'e'];
console.log(arr.push('f', 'g'));			// 7
console.log(arr);				// ["a", "b", "c", "d", "e", "f", "g"] 改变原数组
```

##### pop
pop()数组中删除最后一个元素, 并返回该元素的值, 当数组为空时返回```undefind```, 此方法更改数组的长度
```javascript
let arr = [1, 2, 3, 4, 5];
console.log(arr.pop());				// 5
console.log(arr);							// [1, 2, 3, 4] 改变原数组
```

##### shift
shift()从数组中删除第一个元素, 并返回该元素的值, 该方法会改变原数组
```javascript
let arr = [1, 2, 3, 4, 5]
console.log(arr.shift());			// 1
console.log(arr);							// [2, 3, 4, 5] 改变原数组
```

##### unshift
```arr.unshift(element1[, ..., elementN])```
unshift()将一个或多个元素添加到数组的开头, 并返回该数组的长度, 该方法修改原有数组
```javascript
let arr = [1, 2, 3, 4, 5]
console.log(arr.unshift(-1, 0));			// 7
console.log(arr);			// [-1, 0, 1, 2, 3, 4, 5] 改变原数组
```

##### splice
```arrar.splice(start[, deleteCount[, item1[, item2[, ...]]]])```
```start```指定修改的开始位置, 如果超出了数组的长度, 则从数组末尾开始添加内容; 如果是负值, 则表示从数组末尾开始的第几位(从-1计数, 这意味着-n是倒数第n个元素并且等价于array.length-1); 如果负数的绝对值大于数组的长度, 则表示开始位置为第0位
```deleteCount```可选[整数], 表示要移除的数组元素的个数. 如果deleteCount大于start之后元素的总数, 则从statr后面的元素都将被删除(含第start位). 如果deleteCount被省略, 或者它的值大于等于```array.length-start```(也就是说, 如果它大于或者等于start之后的所有元素的数量), 那么start之后数组的所有元素都会被删除
```item1, item2, ...```可选[要添加进数组的元素, 从start位置开始. 如果不指定, 则splice()将只删除数组元素]
splice()通过删除或替换现有元素或者原地添加新的元素来修改数组, 并以数组形式返回被修改的内容, 此方法会改变原数组
```javascript
let arr = [1, 2, 3, 4, 5]
console.log(arr.splice(1, 1));			// [2]
console.log(arr);										// [1, 3, 4, 5] 改变原数组
```

##### slice
```arr.slice([begin[, end]])```
```begin```可选[提取起始处的索引] 从该索引开始提取原数组元素. 如果该参数为负数, 则表示从原数组中的倒数第几个元素开始提取, 如果省略```begin```, 则```slice```从索引0开始; 如果```begin```大于原数组的长度, 则会返回空数组
```end```可选[提取终止处的索引], 在该索引处结束提取原数组元素. slice会提取原数组中索引从```begin```到```end```到所有元素, 包含```begin```, 但不包含```end```. 如果```end``` 被省略, 则slice会一直提取到原数组末尾, 如果```end```大于数组的长度, slice也会一直提取到数组末尾
slice()返回一个新的数组对象, 这一对象是一个由```begin```和```end```决定到原数组的浅拷贝, 包括```begin```, 不包括```end```, 原始数组不会被改变
```javascript
let arr = [1, 2, 3, 4, 5];
console.log(arr.sclice(1, 3));			// [2, 3]
console.log(arr);										// [1, 2, 3, 4, 5] 不改变原数组
```

##### concat
```let new_array = old_array.concat(value[, value2[, ...[, valueN]]])```
```valueN```可选[], 将数组或值连接成新数组, 如果省略了```valueN```参数, 则concat会返回一个它所调用的已存在的数组的浅拷贝
concat()用于合并两个或多个数组, 此方法不会更改现有数组, 而是返回一个新数组
```javascript
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];
let arr3 = arr1.concat(arr2);

console.log(arr3);					// [1, 2, 3, 4, 5, 6]
console.log(arr1);					// [1, 2, 3] 不改变原数组
```

##### join
```arr.join([separator])```
```separator```可选 指定一个字符串来分隔数组的每个元素
join()将一个数组(或一个类数组对象)的所有元素连接成一个字符串并返回这个字符串. 如果数组只有一个项目, 那么将返回该项目而不使用分隔符
```javascript
let arr = ['a', 'b', 'c'];
console.log(arr.join("&"));					// a&b&c
console.log(arr);										// ["a", "b", "c"] 不改变数组
```

##### sort
```arr.sort([compareFunction])```
```compareFunction```可选 用来指定按某种顺序进行排列的函数. 如果省略, 元素按照转换为第字符串的各个字符的```Unicode```进行排序
```firstEl```第一个用于比较的元素
```secondEl```第二个用于比较的元素
sort()用原地算法对数组的元素进行排序, 并返回数组. 默认排序顺序是在将元素转换为字符串, 然后比较它们的```UTF-16```代码单元值序列时构建的
```javascript
let arr = [1, 2, 3, 4, 5];
console.log(arr.sort((a, b) => b - a));			// [5, 4, 3, 2, 1]
console.log(arr);											// [5, 4, 3, 2, 1] 改变原数组
```

##### reverse
reverse()将数组中元素的位置颠倒, 并返回该数组, 该方法会改变原数组
```javascript
let arr = [1, 2, 3, 4, 5];
console.log(arr.reverse());						// [5, 4, 3, 2, 1]
console.log(arr);											// [5, 4, 3, 2, 1] 改变原数组
```
##### every
every()测试一个数组内的所有元素是否都能通过某个指定函数的测试, 返回一个布尔值
```javascript
let arr = [1, 2, 3, 4, 5];
console.log(arr.every(currentValue => currentValue > 1));			// false
console.log(arr);											// 不改变原数组
```
##### some
some()测试数组中是不是至少有1个元素通过了提供的测试函数, 返回一个```Boolean```类型的值
```javascript
let arr = [1, 2, 3 ,4 ,5];
console.log(arr.some(currentValue => currentValue > 1));			// true
console.log(arr);											// [1, 2, 3, 4, 5] 不改变原数组
```
##### filter
filter()创建一个新数组, 其包含通过所提供的测试函数的所有元素
```javascript
let arr = [1, 2, 3, 4, 5];
console.log(arr.filter(currentValue => currentValue > 2));	// [3, 4, 5]
console.log(arr);											// [1, 2, 3, 4, 5] 不改变原数组
```

##### find
find()返回数组中满足提供的测试函数的第一个元素的值, 否则返回```undefined```
```javascript
let arr = [1, 2, 3, 4, 5];
console.log(arr.find(currentValue => currentValue > 2));		// 3
console.log(arr);											// [1, 2, 3, 4, 5] 不改变原数组
```
##### findIndex
findIndex()返回数组中满足提供的测试函数的第一个元素的索引, 否则返回-1
```javascript
let arr = [1, 2, 3, 4, 5];
console.log(arr.findIndex(currentValue => currentValue > 2));		// 2
console.log(arr);											// [1, 2, 3, 4, 5] 不改变原数组
```
##### includes
includes()用来判断一个数组是否包含一个指定的值, 如果包含则返回true, 否则返回false
```javascript
let arr = [1, 2, 3, 4, 5];
console.log(arr.includes(2));					// true
console.log(arr);											// [1, 2, 3, 4, 5] 不改变原数组
```
##### indexOf
indexof()返回指定元素在数组中的第一个索引, 否则返回-1
```javascript
let arr = [1, 2, 3, 4, 5];
console.log(arr.indexOf(2));					// 1
console.log(arr);											// [1, 2, 3, 4, 5] 不改变原数组
```
##### lastIndexOf
lastIndexOf()返回指定元素在数组中的的最后一个索引, 如果不存在则返回-1
```javascript
let arr = [1, 2, 3, 2, 1];
console.log(arr.lastIndexOf(2));			// 3
console.log(arr);											// [1, 2, 3, 2, 1] 不改变原数组
```

##### fill
fill()用一个固定值填充一个数组从起始索引到终止索引到全部元素, 不包括终止索引
```javascript
let arr = [1, 2, 3, 4, 5];
console.log(arr.fill(0, 0, 5));				// [0, 0, 0, 0, 0]
console.log(arr);											// [0, 0, 0, 0 ,0] 改变数组
```

##### flat
flat()会按照一个可指定的深度递归遍历数组, 并将所有元素与遍历到的子数组中的元素合并为一个新数组返回
```javascript
let arr = [1, 2, [3, 4, [5]]];
console.log(arr.flat(2));							// [1, 2, 3, 4, 5]

console.log(arr);											// [1, 2, [3, 4, [5]]] 不改变原数组
```

##### keys
keys()返回一个包含数组中每个索引键的```Array Iterator```对象
```javascript
let arr = [1, 2, 3, 4, 5];
let iterator = arr.keys();

for (const key of iterator) {
  console.log(key);
  // 0
  // 1
  // 2
}

console.log(arr);												// [1, 2, 3, 4, 5] 不改变原数组
```
#### 常用操作

##### 数组去重
使用对象
```javascript
let arr = [1, 2, 3, 1, 1, 1, 3, 5, 3];
let obj = {};
let newArr = [];
arr.forEach(v => {
  if(!ogj[v]) {
    ogj[v] = 1;
    newArr.push(v);
  }
})

console.log(newArr);										// [1, 2, 3, 5]
```

使用Set
```javascript
let arr = [1, 2, 3, 1, 1, 1, 3, 5, 3];
let newArr = Array.from(new Set(arr));
// let newArr = [...(new Set(arr))];		// 使用ES6解构赋值

console.log(newArr);										// [1, 2, 3, 5]
```
##### 扁平化数组
使用flat
```javascript
let arr = [1, 2, [3, 4, [5]]];
let newArr = arr.flat(2);

console.log(newArr);										// [1, 2, 3, 4, 5]
```

递归实现flat
```javascript
function _flat(arr, maxN = 1, curN = 0) {
  let newArr = [];
  if (curN >= maxN) return arr;
  arr.forEach((v, i, array) => {
    if (Array.isArray(v)) {
      newArr.push(..._flat(v, maxN, curN + 1));
    } else newArr.push(v);
  })
  return newArr;
}

let arr = [1, 2, [3, 4, [5]]];
let newArr = _flat(arr, 1);							// 扁平化一层

console.log(newArr);										// [1, 2, 3, 4, [5]]
```

##### 统计一个字符串中出现最多的字符
使用数组将字符的```ASCII```码作为```key```制作桶
```javascript
let s = "ASASRKIADAA";
let arr = [];
let base = 65;													// A-Z 65-90 a-z 97-122
Array.prototype.forEach.call(s, (v) => {
  let ascii = v.charCodeAt(0) - base;
  if (arr[ascii]) {
    ++arr[ascii];
  } else arr[ascii] = 1;
})

let max = 0;
let maxIndex = 0;
arr.forEach((v, i) => {
  if (v > max) {
    max = v;
    maxIndex = i;
  }
})

console.log(String.fromCharCode(maxIndex + base), arr[maxIndex]);	// A 5
```

##### 找出数组中的最大值
遍历数组
```javascript
let arr = [1, 2, 3, 1, 1, 1, 3, 5, 3];
let max = -Infinity;
arr.forEach(v => {
  if (v > max) max = v;
})

console.log(max);										// 5
```

使用```Math```
```javascript
let arr = [1, 2, 3, 1, 1, 1, 3, 5, 3];
let max = Math.max(...arr);
console.log(max);											// 5
```

使用```reduce```
```javascript
let arr = [1, 2, 3, 1, 1, 1, 3, 5, 3];
let max = arr.reduce((a, v) => {
  return a > v ? a : v;
})

console.log(max);											// 5
```

##### 拷贝数组
遍历数组使用```push```
```javascript
let arr = [1, 2, 3, 4, 5];
let newArr = [];
arr.forEach(v => newArr.push(v));

console.log(newArr);									// [1, 2, 3, 4, 5]
```

使用```concat```
```javascript
let arr = [1, 2, 3, 4, 5];
let newArr = [].concat(arr);
console.log(newArr);									// [1, 2, 3, 4, 5]
```
使用```slice```
```javascript
let arr = [1, 2, 3, 4, 5];
let newArr = arr.slice(0);
console.log(newArr);									// [1, 2, 3, 4, 5];
```

##### 随机打乱一个数组
随机交换N次
```javascript
function randomInt(a, b) {
  return Number.parseInt(Math.random() * (b-a) + a);
}

let arr = [1, 2, 3, 4, 5];
let N = arr.length;
arr.forEach((v, i, arr) => {
  let ran = randomInt(0, N);
  [arr[i], arr[ran]] = [arr[ran], arr[i]];
})

console.log(arr);
```

随机排序
```javascript
let arr = [1, 2, 3, 4, 5];
arr.sort((v1, v2) => {
  return Math.random() >= 0.5 ? 1 : -1;
})

console.log(arr);
```