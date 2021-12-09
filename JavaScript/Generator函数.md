### Generator函数
生成器```Generator```是```ES6```标准引人的新数据类型, 一个```Generator```看上去像一个函数, 但可以返回多次, 通过```yield```关键字, 将函数的执行流程挂起, 为改变函数执行流程提供了可能, 是一种异步编程解决方案

#### 方法
- ```Generator.prototype.next()```: 返回一个由```yield```表达式生成的值
- ```Generator.prototype.return()```: 返回给定的值并结束生成器
- ```Generator.prototype.throw()```: 向生成器抛出一个错误

### 实例
使用```function*```声明方式会定义一个生成器函数```generator function```, 它返回一个```Generator```对象, 可以将```Generator```函数理解为一个状态机器, 封装了多个内部状态,  执行```Generator```函数会返回一个遍历器对象
调用一个生成器函数并不会马上执行它里面的语句, 而是返回一个生成器的迭代器```iterator```对象, 它是一个指向内部状态对象的指针. 当这个迭代器的```next()```方法后续被调用时, 其内部的语句会执行到第一个出现```yield```到位置为止, ```yield```后紧跟迭代器要返回到值, 也就是指针会从函数头部或者上一次停下来的开始执行到下一个```yield```. 如果使用```yield*```, 则表示将执行权移交给另一个生成器函数(当前生成器暂停执行)
```next()```方法返回一个对象, 这个对象包含两个属性: ```value```和```done```, ```value```属性表示本次```yield```表达式的返回值, ```done```属性为布尔类型, 表示生成器后续是还有```yield```语句, 即生成器函数是否已经执行完毕并返回
```javascript
function* f(x) {
  yield x + 10;
  yield x + 20;
  yield x + 30;
}

const g = f(1);
console.log(g);						// f {<suspended>}
console.log(g.next());		// {value: 11, done: false}
console.log(g.next());		// {value: 21, done: false}
console.log(g.next());		// {value: 31, done: true}
console.log(g.next());		// {value: undefined, done: true}
```

调用```next()```方法时, 如果传入了参数, 那么这个参数会传给上一条执行的```yield```语句左边的变量
```javascript
function* f(x) {
  let y = yield x + 10;
  console.log(y);
  yield x + y;
  console.log(x, y);
  return x + 30;
}

const g = f(1);
console.log(g);							// f {<suspended}
console.log(g.next());			// {value: 11, done: false}
console.log(g.next(50));		// {value: 51, done: false}
console.log(g.next());			// {value: 31, done: true}
console.log(g.next());			// {value: undefined, done: true}
```

若显示指明```return```方法返回值, 则返回该值并结束遍历```Generator```函数, 若未显示指明```return```的值, 则返回```undefined```
```javascript
function* f(x) {
  yield x + 10;
  yield x + 20;
  yield x + 30;
}

const g = f(1);
console.log(g);							// f {<suspended>}
console.log(g.next());			// {value: 11, done: false}
console.log(g.next());			// {value: 21, done: false}
console.log(g.next());			// {value: 31, done: false}
console.log(g.next());			// {value: undefined, done: true}
```

```yield*```表达式表示```yield```返回一个遍历器对象, 用于在```Generator```函数内部, 调用另一个```Generator```函数
```javascript
function* callee() {
  yield 100;
  yield 200;
  yield 300;
}
function* f(x) {
  yield x + 10;
  yield* callee();
  yield x + 30;
}

const g = f(1);
console.log(g);								// f {suspended}
console.log(g.next());				// {value: 11, done: false}
console.log(g.next());				// {value: 100, done: false}
console.log(g.next());				// {value: 200, done: false}
console.log(g.next());				// {value: 31, done: false}
console.log(g.next());				// {value: undefined, done: true}
```

#### 异步操作的同步化表示
```javascript
let it = null;

function f() {
  let rand = Math.random() * 2;
  setTimeout(function() {
    if(it) {
      it.next(rand);
    }
  }, 1000)
}
function success(r1, r2, r3) {
  console.log(r1, r2, r3);
}
// 成为线性任务而解决嵌套
function* g() {
  let r1 = yield f();
  console.log(r1);
  let r2 = yield f();
  console.log(r2);
  let r3 = yield f();
  console.log(r3);
  console.log(r3);
  success(r1, r2, r3);
}

it = g();
it.next();
```