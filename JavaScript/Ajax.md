###  Ajax
Ajax技术的核心是`XMLHttpRequest`对象(简称XHR), XHR为向服务器发送请求和解析服务器响应提供了流畅的接口, 能过以异步的方式从服务器获取数据, 再通过DOM将新数据插入到页面中, 从而使得不用刷新页面也能获得新数据

#### XMLHttpRequest对象
现在的主流浏览器都支持原生的XHR对象, 如果要支持IE的早期版本(即IE7之前的版本), 那么可以这么写:
```javascript
let createXHR = () = > {
  // 首先检测原生XHR对象是否存在
  if (typeof XMLHttpRequest != "undefined") {
    return new XMLHttpRequest();
    // 如果不存在原生对象, 则检测Active对象
  } else if (typeof ActiveXObject != "undefined") {
    if (typeof arguments.callee.activeXString != "string") {
      let versions = [ "MSXML2.XMLHttp.6.0", "MSXML2.XMLHttp.3.0",
                       "MSXML2.XMLHttp"], i, len;
      for (i = 0; len = versions.length; i < len; i++) {
        try {
          new ActiveXObject(versions[i]);
          argumnets.callee.activeXString = versions[i];
          break;
        } catch (ex) { console.log("Request was unsuccessful: " + xhr.status); }
      }
    }
    
    return new ActiveXObject(arguments.callee.activeXString);
  } else {
    throw new Error("No XHR object avaiable.");
  }
}
```

####  XHR的用法
在使用XHR对象时, 要调用的第一个方法是open(), 它接收五个参数: 要发送的请求的类型("get", "post"等), 请求的URL, 表示书否异步发送请求的布尔值, 以及最后两个可选参数: 用以服务端验证的账号密码(默认为null)
```javascript
xhr.open("get", "example.php", fasle);
```
调用open函数后, 有两点需要注意:
  - 1. URL相对于执行代码的当前页面(也可以是 绝对路径)
  - 2. 调用open()方法并不会真正发送请求, 只是启动了一个请求以备发送

要发送特定的请求, 必须调用send()方法
```javascript
xhr.open("get", "example.txt", false);
xhr.send(null);
```
这里的send()方法接收一个参数, 即要作为请求主体发送的数据. 如果不需要通过请求主题发送数据, 则必须传入null, 因为这个参数有对有些浏览器来说是必须的. 调用send()之后, 请求就会被分派到服务器.
当将open()的是否异步设置为`fasle`时, 代表此次请求是同步的, javascript代码会等到服务器响应之后再继续执行. 在收到响应后, 响应的数据会自动填充XHR对象的属性
  - responseText: 作为响应主体被返回的文本
  - responseXML: 如果响应的内容类型是"text/xml"或"application/xml", 这个属性中将保存包含着响应数据的XML DOM文档
  - status: 响应的HTTP状态
  - statusText: HTTPZ状态的说明

在接收到响应后, 第一步是检查status属性, 以确定响应已经成功返回. 一般来说, 可以将HTTP状态代码为`200`作为成功的标志. 此时responseText属性的内容已经就绪, 而且在内容类型正确的情况下, responseXML也应该能够访问了. 此外, 状态码为`304`表示请求的资源并没有被修改, 可以直接使用浏览器中缓存的版本. 为了确保能够接收到适当的响应, 应该这样检查上述两种状态码
```javascript
xhr.open("get", "example.txt", false);
xhr.send(null);

if (xhr.status >= 200 && xhr.status < 300) || xhr.status == 304 {
  // some code
} else {
  console.log("Request was unsuccessful: " + xhr.status);
}
```
通过检测status来决定下一步的操作, 无论内容类型是什么, 响应主体的内容都会保存到responseText属性中; 而对于非XML数据而言, responseXML属性的值将为null

当open()方法设定发送的方式为异步请求时, `onreadystatecharge`事件监听器将自动在`XMLHttpRequest`对象的`readyState`属性改变时被触发, 该属性表示请求/响应过程的当前活动阶段, 这个属性的可取值如下:
  - 0: 未初始化. 尚为调用open()方法
  - 1: 启动. 已经调用open()方法, 但尚未调用send()方法
  - 2: 发送. 已经调用send()方法, 但尚未接收到响应
  - 3: 接收. 已经接收到部分响应数据
  - 4: 完成. 已经接收到全部响应数据, 而且已经可以在客户端使用了

只要readyState属性的值由一个值变成另一个值, 都会触发一次readystatechange事件. 在调用open()之前指定onreadystatechange事件处理程序可以确保浏览器兼容性
```javascript
let xhr = createXHR();
xhr.onreadystatechange = () => {
  if (xhr.readyState == 4) {
    if (xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
      // some code
    } else {
      console.log("Request was unsuccessful: " + xhr.status);
    }
  }
};
xhr.open("get", "example.txt", true);
xhr.send(null);
```
在接收到响应之前可以调用`abort()`方法来取消异步请求
```javascript
xhr.abort();
```

#### HTTP头部信息
默认情况下, 在发送XHR请求的同时, 还会发送下列头部信息
  - Accept: 浏览器能够处理的内容类型
  - Accept-Charset: 浏览器能够显示的字符集
  - Accept-Encoding: 浏览器能够处理的压缩编码
  - Accept-Language: 浏览器当前设置的语言
  - Conection: 浏览器与服务器之间连接的类型
  - Cookie: 当前页面设置的任何Cookie
  - Host: 发出请求的页面所在的域
  - Referer: 发出请求的页面的URL.(小彩蛋, HTTP规范将这个头部字段拼写错了, 正确的拼写方法应该是referrer)
  - User-Agent: 浏览器的用户代理字符串
使用setRequestHeader()方法可以设置自定义的请求头部信息. 这个方法接收两个参数: 头部字段的名称和头部字段的值. 要成功发送请求头部信息, 必须调用`open()`方法之后且调用`send()`方法之前调用`setRequestHeader()`
```javascript
let xhr = createXHR();
xhr.onreadystatechange = () => {
  if (xhr.readyState == 4) {
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
      // some code
    } else {
      console.log("Request was unsuccessful: " + xhr.status);
    }
  }
}
xhr.open("get", "example.php", true);
xhr.setRequestHeader("MyHeader", "MyValue");
xhr.send(null);
```
调用XHR对象的`getResponseHeader()`方法并传入头部字段名称, 可以取得相应的响应头部信息. 而调用`getAllResponseHeaders()`方法则可以取得一个包含所有头部信息的长字符串
```javascript
let myHeader = xhr.getResponseHeader("MyHeader");
let allHeaders = xhr.getAllResponseHeaders();
```

#### ajax的简单实现
```javascript
let ajax = (url, method="GET", data = null, async = true) => {
  // 声明XMLHttpRequest
  let xhr = new XMLHttpRequest();
  xhr.onreadystatechange = () => {
    if (xhr.readyState === 4) {
      console.log(`响应状态: ${xhr.status}`, "FINISH");
    }
    // 初始化请求参数
    xhr.open(method, url, async);
    // 发起请求
    xhr.send(data);
  }
}

ajax("https://www.google.com");
ajax("https://www.google.com", "POST", "A=1&B=2");
```