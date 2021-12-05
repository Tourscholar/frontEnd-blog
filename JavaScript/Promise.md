###  Promise
#### 什么是Promise
JavaScript是单线程的语言, 通过维护执行栈与任务队列来实现异步操作, `setTimeout`和`Ajax`就是典型的异步操作
```javascript
setTimeout((res) => { console.log(res) }, 1000)
```
但是当多个使用回调函数所实现的异步函数一个接着一个地执行时, 就会产生`回调地狱`, 此时的代码会变得难以理解, 这是由回调函数的嵌套而引起的
`Promise`就是异步编程的一种解决方案, 它最早由社区提供, ES6将其写进了语言标准, 统一了用法, 原生提供了`Promise`对象

#### Promise对象的两个特点
  - 1. Promise对象的状态不受外界影响. Promise对象代表一个异步操作, 有三种状态: `pending`(进行中), `fulfilled`(已成功), `rejected`(已失败). 只有异步操作的结果可以决定当前是哪一种状态, 任何其他操作都无法改变这个状态
  - Promise对象的状态一旦改变就不再变了, 任何时候都可以得到这个结果. Promise对象的状态改变, 只有两种可能: 从`pending`变为`fulfilled`和从`pending`变为`rejected`(已失败). 只要这两种情况发生, 状态就凝固了, 不会再变了, 会一直保持这个解结果, 这时就称为`resolved`(已定型). 如果改变已经发生了, 再对Promise对象添加回调函数, 页会立即得到这个结果

Promise也有一些缺点: 首先, 无法取消Promise, 一旦新建它就会立即执行, 无法中途取消; 其次, 如果不设置回调函数, Promise内部抛出的错误不会反应到外部, 如果不主动`catch`捕获Promise内部抛出的异常, 则在`FireFox`中异常不会抛出, 在`Chrome`中的异常会抛出但不会触发`window.onerror`事件; 当处于`pending`状态时, 无法得知目前进展到哪一个阶段

有了Promise对象, 就可以将异步操作以同步操作的流程表达出来, 避免了层层嵌套的回调函数. 此外Promise对象提供统一的接口, 使得控制异步操作更加容易

#### 基本用法
Promise对象是一个构造函数, 用来生成Promise实例
```javascript
const promise = new promise((resole, reject) => {
  ...
  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```
Promise构造函数接受一个函数作为参数, 该函数的两个参数分别是`resolve`和`reject`. 它们是两个函数: ```resolve```函数的作用是将Promise对象的状态从"未完成"变为”完成"(即从pending变为resolved), 在异步操作成功时调用, 并将异步操作的结果作为参数传递出去; ```reject```函数的作用是将Promise对象的状态从"未完成"变为"失败"(即从pending变为rejected), 在异步操作失败时调用, 并将异步操作报出的错误, 作为参数传递出去

Promise实例生成以后, 可以用```then```方法分别指定```resolved```状态和```rejected```状态的回调函数
```javascript
promise.then((value) => {
  // success
}, (error) => {
  // failure
});
```
```then```方法可以接收两个回调函数作为参数, 第一个回调函数在当Promise对象的状态变为```resolved```时调用, 第二个回调函数在当Promise对象的状态变为```rejected```时调用. 这两个函数都是可选的, 它们都接收Promise对象传出的值作为参数
`例子`
```javascript
let promise = new Promise((resolve, reject) => {
  let rand = Math.rnadom() * 2;
  setTimeout(() = > {
    if (rand < 1) resolve(rand);
    else reject(rnad);
  }, 1000)
});
promise.then((rand) => {
  console.log("resolve", rand);		// 接收rand参数, resolve回调执行
}).catch(() => {
  console.log("reject", rand);		// 接收rand参数, reject回调执行
})
```
Promise新建后会立即执行, 会首先执行创建的在new Promise对象中的语句, 然后`then`方法指定的回调函数, 将在当前脚本所有同步任务执行完才会执行
```javascript
let promise = new Promise((resolve, reject) => {
  console.log("Promise");
  resolve();
});

promise.then(() => {
  console.log("resolved");
});

conlose.log("Hi!");
// Promise
// Hi!
// resolved
```
如果调用```resolve```函数和```reject```函数时带有参数, 那么它们的参数会被传递给回调函数. ```reject```函数的参数通常是```Error```对象是实例, 表示抛出的错误; ```resolve```函数的参数除了正常的值以外, 还可能是另一个Promise实例
```javascript
const p1 = new Promise((resolve, reject) => {
  // ...
});
const p2 = new Promise((resolve, reject) => {
  // ...
  resolve(p1);		// p1的状态传递给了p2, 即p1的状态决定了P2的状态
       // 如果p1的状态是pending, 那么p2的回调函数就会等待p1的状态改变
  // 如果p1的状态已经是resolved或者rejected, 那么p2的回调函数就会执行
});
```
```javascript
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => reject(new Error("fail")), 3000);
});
const p2 = new Promise((resolve, reject) => {
  setTimeout(() => resolve(p1), 1000);
});

p2.then(result => console.log(result))
  . catch(error => console.log(error))
```
p1和p2都是Promise,由于 p2接收了p1的状态, 所以p2本身的状态无效了, 由p1的状态决定p2的状态, 所以后面的`then`语句都变成针对p1

#### Promise.prototype.then()
Promise实例具有`then`方法, 也就是说`then`方法是定义在原型对象`Promise.prototype`上的. 它的作用是为Promise实例添加状态改变时的回调函数
`then`方法返回的是一个新的Promise实例(不是原来的那个Promise实例)
```javascript
getJson("/post/1.json").then(
  post => getJSON(post.commentURL)
).then(
  comments => console.log("resolved: ", comments),
  err => console.log("rejected: ", err);
);
```

#### Promise.prototype.catch()
`Promise.prototype.catch()`方法是`.then(null, rejection)`或`.then(undefined, rejecion)`的别名, 用于指定发生错误时的回调函数
```javascript
getJSON('/posts.json').then((posts) => {
  // ...
}).catch((error) => {
  // 处理getJSON和前一个回调函数运行时发生的错误
  console.log("发生错误", error);
});
```

如果Promise状态已经变成`resolved`, 再抛出错误是无效的
```javascript
const promise = new Promise((resolve, reject) => {
  resolve("ok");
  throw new Error("test");
});
```

Promise对象的错误具有"冒泡"性质, 会一直向后传递, 直到被捕获为止. 也就是说, 错误总是会被下一个`catch`语句捕获
```javascript
getJSON("/post/1.json").then((post) => {
  return getJSON(post.commentURL);
}).then((comments) => {
  // some code
}).catch((error) => {
  // 处理前面三个Promise产生的错误
})
```

#### Promise实现Ajax
```javascript
let ajax = (method, url, data) => {
  let request = new XMLHttpRequest();
  return new Promise((resolve, reject) => {
    request.onreadystatechange = () => {
      if (request.readyState === 4) {
        if (request.status === 200) {
          resolve(request.responseText);
        } else {
          reject(request.status);
        }
      }
    };
    request.open(method, url);
    request.send(data);
  });
}

ajax("GET", "https://www.google.com", []).then((res) => {
  console.log(res);
}).catch((err) => {
  console.log(err);
});
```