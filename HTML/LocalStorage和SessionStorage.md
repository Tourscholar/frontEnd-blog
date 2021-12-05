### LocalStorage和SessionStorage
HTML5中的Web Storage包括了两种存储方式: SessionStorage和LocalStorage. SessionStorage用于本地存储一个会话(Session)中的数据, 这些数据只有在同一个会话的页面才能访问, 会话结束后数据也随之销毁. LocalStorage用于存储一个域名下的需要永久存在本地的数据, 这些数据可以被一直访问, 直到这些数据被删除. 因此SessionStorage和LocalStorage的主要区别在于它们存储数据的生命周期, SessionStorage存储的数据的生命周期是一个会话, 而LocalStorage存储的数据的生命周期是永久, 直到被主动删除, 否则这个数据永远不会过期

#### Web Storage和Cookie的异同点
Web Storage和Cookie的相同之处
  - 都可以用于存储用户数据
  - 存储数据的格式都是字符串
  - 存储的数据都有大小限制

Web Storage和Cookie的不同之处
  - 生命周期不同. SessionStorage的生命周期是一个会话, LocalStorage的生命周期是永久, Cookie的生命周期可以自定义, Cookie可以设置过期时间, 数据在过期时间之前可以访问
  - 存储大小限制不同, 大部分现代浏览器Storage的存储限制大小为5M, Cookie的存储大小限制为4K
  - 浏览器支持不同, API调用方式不同

相比Cookie, Web Storage的优点主要表现在存储空间更大, 可存储的内容更大. Cookie每次都随请求数据发送到服务端, Web Storage不会和请求数据一同发送到服务端, 占用带宽更少. 缺点主要表现在: 现在所有浏览器都支持Cookie操作但只有现代浏览器才支持Web Storage操作, 如果需要兼容老旧浏览器, 就不能使用Web Storage

#### Web Storage API
LocalStorage和SessionStorage有着统一的API接口, 这为二者的操作提供了极大的便利.
  - 添加键值对: localStorage.setltem(key, value). ```setItem```用于把值value存储到键key上, 除了使用```setItem```, 还可以使用```localStorage.key = value```或者```localStorage["key"] = value```这两种方式. 需要注意的是, key和value值必须是字符串形式的, 如果不是字符串, 会调用它们相应的```toString()```方法来转换成字符串再存储. 当我们要存储对象时, 应先转换成我们可以识别的字符串格式(比如JSON格式)再进行存储
```javascript
// 把一个用户名存储到name键上
localStorage.setItem("name", "alex");
// localStorage.name = "alex";
// localStorage["name"] = "alex";
//把一个用户存储到user键上
localStorage.setItem("user", JSON.stringfy(id:1, name:"alex"));
```
  - 获取键值: localStorage.getItem(key). ```getItem```用于获取键key对应的数据, 和```setItem```一样, ```getItem```也有两种等效形式: ```value = loaclStorage.key```和```value = localStorage["key"]```. 获取到的value值是字符串类型, 如果需要其他类型, 要做手动的类型转换
```javascript
// 获取存储到name上的值
let name = localStorage.getItem("name");
// let name = localStorage.name;
// let name = localStorage["name"];
// 获取存储到user上的值
let user = JSON.parse(localStorage.getItem("user"));
```
  - 删除键值对: localStorage.removeItem(key). ```removeItem```用于删除指定键的项, localStorage没有数据过期的概念, 所有数据如果失效了, 需要开发者手动删除
```javascript
let name = localStorage.getItem("name");
// 删除存储到name的键上的值
localStorage.removeItem("name");
name = localStorage.getItem("name");	// null
```
  - 清除键值对: localStorage.clear(). ```clear```用于删除所有存储的内容, 它和```removeItem```不同的地方是```removeItem```删除的是某一项, 而```clear```是删除所有
```javascript
// 清除localStorage
localStorage.clear();
let len = localStorage.length;	// 0
```
  - 获取localStorage的属性名称(键名称): localStorage.key(index). ```key```方法用于获取指定索引的键的名称. 需要注意的是赋值早的键对应的索引值大, 赋值晚的键值对应的索引小, ```key```方法用于遍历localStorage存储的键值
```javascript
localStorage.setItem("name", "alex");
let key = localStorage.key(0);		// "name"
localStorage.setItem("age", 10);
key = localStorage.key(0);		// "age"
key = localStorage.key(1);		// "name"
```
  - 获取localStorage中保存的键值对的数量: localStorage.length. ```length```属性用于获取localStorage中键值对的数量
```javascript
localStorage.setItem("name", "alex");
let len = localStorage.len;		// 1
localStorage.setItem("age", 10);
len = localStorage.len;		// 2
```