### JavaScript 观察者模式
观察者模式```Observer Pattern```时一种对象行为模式, 当定义的对象间有一种一对多依赖关系, 使得每当一个对象状态发送改变时, 其相关依赖对象皆得到通知并被自动更新. 观察者模式又叫做"发布-订阅"```Publish-Subscribe```模式, "模型-视图"```Model-View```模式, "源-监听器"```Source-Listener```模式或从属者```Dependents```模式

观察者模式建立了一种对象与对象之间的依赖关系, 一个对象发生改变时将自动通知其他对象, 其他对象将相应做出反应. 所以发生改变的对象称为观察目标, 而被通知的对象称为观察者, 一个观察目标可以对应多个观察者, 而且这些观察者之间没有相互联系, 可以根据需要增加和删除观察者, 使得系统更易于扩展

#### 优点
  - 观察者模式可以实现表示层和数据逻辑层的分离, 并定义了稳定的消息更新传递机制, 抽象了更新接口, 使得可以有各种各样不同的表示层作为具体观察者角色
  - 观察者模式在观察目标和观察者之间建立一个抽象的耦合
  - 观察者模式支持广播通信
  - 观察者模式符合开闭原则的要求

#### 缺点
  - 如果一个观察目标对象有很多直接和间接的观察者的话, 将所有的观察者都通知到会花费很多时间
  - 如果在观察者和观察目标之间有循环依赖的话, 观察目标会触发它们之间的循环调用, 可能导致系统崩溃
  - 观察者模式没有相应的机制让观察者直到所视察的目标对象是怎么发送变化的, 而仅仅只是知道观察目标发生了变化

#### 适用环境
  - 一个抽象模型有两个方面, 其中一个方面依赖于另一个方面. 将这些方面封装在独立的对象中使它们可以各自独立地改变和复用
  - 一个对象的改变将导致其他一个或多个对象也发生改变, 而不知道具体有多少对象将发生改变, 可以降低对象之间的耦合度
  - 一个对象必须通知其他对象, 而并不知道这些对象是谁
  - 需要在系统中创建一个触发链, A对象的行为将影响B对象, B对象的行为将影响C对象……，可以使用观察者模式创建一种链式触发机制

#### 实现
```javascript
class Publish {
  constructor() {
    this.handler = {};
  }
  
  // 订阅
  on(key, handler) {
    if (!(key in this.handlers)) { this.handlers[key] = []; }
    this.handlers[key].push(handler);
  }
  
  // 移除
  off(key, handler) {
    const index = this.handler[key].findIndex(item => item === handler);
    if (index < 0) return false;
    if (this.handler[key].length === 1) { delete this.handler[key]; }
    else { this.handlers[key].splice(index, 1); }
    
    return true;
  }
  
  // 触发
  commit(key, ...args) {
    if (!this.handlers[key]) { return false; }
    this.handlers[key].forEach(handler => handler.apply(this, args));
    
    return true;
  }
}

const eventBus = new PubSub();

// 实例

class JobBoard {
  subscribe(funct) {
    eventBus.on("job-sub", funct);
  }
  
  notify() {
    eventBus.commit("job-sub");
  }
}


class JobSeeker {
  constructor(name) {
    this._name = name;
  }
  
  notify() {
    console.log(this._name, "this been notified of a new posting");
  }
}

(function() {
  let jonDoe = new JobSeeker("John Doe");
  let janeDoe = new JobSeeker("Jane Doe");
  let kaneDoe = new JobSeeker("Kane Done");
  
  let jobBoard = new JobBoard();
  joBoard.subscribe( () => jonDoe.notify());
  joBoard.subscribe( () => janeDoe.notify());
  joBoard.subscribe( () => kaneDoe.notify());
})();
```

