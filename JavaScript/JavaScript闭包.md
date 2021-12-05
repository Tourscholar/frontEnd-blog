### JavaScript闭包
函数对其词法环境```lexical enviroment```的引用捆绑在一起构成闭包, 也就是说, 闭包可以从内部访问函数内部有的的变量. 在JavaScript中, 函数在每次创建时生成闭包. 在本质上闭包是将函数内部和函数外部连接起来的桥梁

#### 定义闭包
闭包需要使用局部变量, 定于使用全局变量就失去了使用闭包的意义, 最外层定义的函数可实现局部作用域从而定义局部变量, 函数外部无法直接访问内部定义的变量
```javascript
let student = () => {
  let name = "Ming";
  let sayMyNane = () => {	// sayMyName作为内部函数, 有权访问父级函数作用域student的变量
    console.log(name);
  };
  return sayMyNane;
};

let stu = student();
stu();
```
定义在函数内部的```name```变量并没有被销毁, 仍然可以在外部使用函数访问这个局部变量, 使用闭包可以把局部变量驻留在内存中, 从而避免使用全局变量.

#### 词法环境
闭包共享相同的函数定义, 但是保存了不同的词法环境
```javascript
let student = name => {
  let sayMyName = () => {
    console.log(name);
  }
  return sayMyName;
}

let stu1 = student("Ming");
let stu2 = student("Yang");
stu1();		// Ming
stu2();		// Yang
```

#### 模拟私有方法
在面向对象的语言中, 例如```Java```、```PHP```等, 都是支持定义私有成员的, 即只有类内部能够访问, 而无法被外部类访问. JavaScript并未原生支持定义私有成员, 但是可以使用闭包来模拟实现, 私有方法不仅仅有利于限制对代码的访问, 还提供了管理全局命名空间的强大能力, 避免非核心的方法弄乱了代码的公共接口部分
```javascript
let student = () => {
  let HP = 100;
  let addHP = () => {
    return HP++;
  }
  let decHP = () => {
    return HP--;
  }
  
  return {
    addHP,
    decHP
  };
}

let stu = student();
console.log(stu.HP);		// undefined 不允许直接访问
console.log(stu.addHP());	// 101
console.log(stu.decHP());	// 100
```

#### 回调机制
JavaScript的闭包为回调机制提供了支持, 无论函数是否立马被调用, 这个闭包都不会被释放. 而且无论把```callback```函数作为参数传递给其他函数或者作为返回值返回, 以便于之后调用, 都是合法的
```javascript
let localContext = () => {
  let localVal = 1;
  let callback = () => {
    console.log(localVal);
  }
  setTimeout(callback, 1000);
}
localContext();
```
```callback```函数与其词法环境构成了闭包, 其词法环境中存在的变量```localVal = 1```在函数```callback```作为回调函数传递时并没有被立即释放, 而可以在回调执行时继续使用, 者就是闭包为回调机制提供了支持

#### 循环创建闭包
在```ECMAScript 2015```引入```let```关键字之前, 只有函数作用域和全局作用域, 函数作用域中又可以继续嵌套函数作用域, 在```for```并未具备局部作用域, 使用```let```关键字可解决该问题
```javascript
let counter = () => {
  let arr = [];
  for (let i = 0; i < 3; ++i) {
    arr[i] = () => {
      return i;
    }
  }
  return arr;
}

let count = counter();
for (let i = 0; i < 3; i++) {
  console.log(count[i]());		// 0 1 2
}
```

#### 内存泄漏
内存泄漏是指用不到(访问不到)的变量, 依然占据着内存空间, 不能被再次利用起来
闭包引用的变量应该是需要使用的, 不应该属于内存泄漏, 但在```IE8```浏览器中```JScript.dll```引擎使用会出现一些问题, 造成内存泄漏

#### 性能考量
如果不是某些特定任务需要使用闭包, 在其他函数中创建函数是不明智的, 因为闭包在处理速度和内存消耗方面对脚本性能具有负面影响. 在创建新的对象或类时, 方法通常应该关联于对象的原型, 而不是定义到对象的构造器中. 原因是着将导致每次构造器被调用时, 方法都会被重新赋值一次